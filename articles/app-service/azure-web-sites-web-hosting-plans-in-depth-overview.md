<properties
	pageTitle="Azure App Service-Pläne – Detaillierte Übersicht | Microsoft Azure"
	description="Erfahren Sie, wie App Service-Pläne für Azure App Service funktionieren, und wie Ihre Verwaltungsumgebung davon profitiert."
	keywords="App-Dienst, Azure App Service, Skalierung, skalierbar, App Services-Plan, App Service-Kosten"
	services="app-service"
	documentationCenter=""
	authors="btardif"
	manager="wpickett"
	editor=""/>

<tags
	ms.service="app-service"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/07/2016"
	ms.author="byvinyal"/>

# Azure App Service-Pläne – Detaillierte Übersicht#

Ein App Service-Plan stellt eine Reihe von Features und Kapazitäten dar, die Sie App-übergreifend (also etwa in Web-Apps, Mobile Apps, Logik-Apps und API-Apps) in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) nutzen können. Für diese Pläne stehen fünf Tarife zur Verfügung: *Free*, *Shared*, *Basic*, *Standard* und *Premium*. Jeder Tarif verfügt über eigene Funktionen und Kapazitäten. Apps im selben Abonnement und am gleichen geografischen Standort können den gleichen Plan verwenden. Sämtliche Apps eines gemeinsamen Plans können alle Funktionen und Features nutzen, die durch die Preisstufe des Plans definiert sind. Alle einem Plan zugeordneten Apps werden auf den durch den Plan definierten Ressourcen ausgeführt.

Wenn Ihr Plan also etwa für die Verwendung von zwei kleinen Instanzen der Standard-Preisstufe konfiguriert ist, werden alle Apps, die diesem Plan zugeordnet sind, auf beiden Instanzen ausgeführt und haben Zugriff auf die Funktionen der Standard-Preisstufe. Planinstanzen, auf denen Apps ausgeführt werden, sind vollständig verwaltbar und hochverfügbar.

Dieser Artikel beschäftigt sich mit zentralen Aspekten wie Tarif und Umfang eines App Service-Plans und deren Bedeutung für die App-Verwaltung.

## Apps und App Service-Pläne

Jede App in App Service kann nur einem App Service-Plan gleichzeitig zugeordnet sein.

Apps und Pläne sind in einer Ressourcengruppe enthalten. Ressourcengruppen fungieren als Lebenszyklusgrenze für alle darin enthaltenen Ressourcen. Mit Ressourcengruppen können Sie alle Ihre Ressourcen in einer Anwendung gemeinsam verwalten.

Da eine einzelne Ressourcengruppe mehrere App Service-Pläne enthalten kann, können Sie verschiedene Apps unterschiedlichen physischen Ressourcen zuordnen. Dadurch können Sie beispielsweise getrennte Ressourcen für die Entwicklungs-, Test- und Produktionsumgebung verwenden. Ein Szenario hierfür ist, wenn Sie einen Plan mit eigenen dedizierten Ressourcen Ihren Produktions-Apps und einen zweiten Plan Ihren Entwicklungs- und Testumgebungen zuordnen möchten. Auf diese Weise konkurrieren Auslastungstests für eine neue Version Ihrer Apps nicht um dieselben Ressourcen wie Ihre Produktions-Apps, die für reale Kunden bereitstehen.

Wenn eine einzelne Ressourcengruppe mehrere Pläne enthält, können Sie auch eine Anwendung definieren, die sich über mehrere geografische Regionen erstreckt. Eine hochverfügbare Anwendung in zwei Regionen umfasst z. B. zwei Pläne, je einen pro Region, und eine App pro Plan. In diesem Fall sind alle Kopien der Anwendung einer einzelnen Ressourcengruppen zugeordnet. Dank einer zentralen Ansicht einer Ressourcengruppe mit mehreren Plänen und mehreren Apps können Sie die Integrität der Anwendung komfortabel verwalten, steuern und anzeigen.

## Erstellen eines neuen App Service-Plans im Vergleich zum Verwenden eines vorhandenen Plans

Wenn Sie eine neue App erstellen, empfiehlt sich unter Umständen das Erstellen einer neuen Ressourcengruppe. Wenn es sich bei der App, die Sie erstellen möchten, um eine Komponente einer größeren Anwendung handelt, muss sie allerdings in der Ressourcengruppe erstellt werden, die der größeren Anwendung zugeordnet ist.

Zum Hosten der App können Sie einen vorhandenen App Service-Plan verwenden oder einen neuen Plan erstellen. Das gilt sowohl für ganz neue Apps als auch für Apps, die Teil einer größeren Anwendung sind. Dies ist eher eine Frage der Kapazität und erwarteten Auslastung.

Wenn die neue App einen hohen Ressourcenbedarf hat und sich ihre Skalierungsfaktoren von denen anderer Apps in einem vorhandenen Plan unterscheiden, empfiehlt es sich, sie in einem eigenen Plan zu isolieren.

Wenn Sie einen neuen Plan erstellen, können Sie Ihrer App einen neuen Satz von Ressourcen zuordnen und die Ressourcenzuordnung besser steuern, da jeder Plan einen eigenen Satz von Instanzen erhält.

Apps können zwischen Plänen verschoben werden, um die Ressourcenzuordnung innerhalb der größeren Anwendung anzupassen.

Wenn Sie schließlich eine neue App in einer anderen Region erstellen möchten und diese Region über keinen vorhandenen Plan verfügt, müssen Sie einen neuen Plan in dieser Region erstellen, damit Sie Ihre App in dieser hosten können.

## Wie erstelle ich einen Plan?

Sie können einen leeren App Service-Plan über die Browseroberfläche für App Service-Pläne oder im Rahmen der App-Erstellung erstellen.

Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Neu** > **Web und mobil** und anschließend auf **Web-App**, **Mobile App**, **API-App** oder **Logik-App**. ![Erstellen einer App im Azure-Portal][createWebApp]

Sie können dann den App Service-Plan für die neue Anwendung auswählen oder erstellen.

 ![Erstellen eines App Service-Plans][createASP]

Klicken Sie zum Erstellen eines neuen App Service-Plans auf **[+] Neu erstellen**, geben Sie den Namen für den **App Service-Plan** ein, und wählen Sie einen geeigneten **Speicherort** aus. Klicken Sie auf **Tarif**, und wählen Sie einen geeigneten Tarif für den Dienst aus. Wählen Sie **Alle anzeigen** aus, um mehr Tarifoptionen anzuzeigen, z. B. **Free** und **Shared**. Klicken Sie nach dem Auswählen des Tarifs auf die Schaltfläche **Auswählen**.

## Wie kann ich eine App in einen anderen App Service-Plan verschieben?

Eine App kann über das [Azure-Portal](https://portal.azure.com) in einen anderen App Service-Plan verschoben werden. Das Verschieben von Apps zwischen Plänen ist möglich, wenn sich die Pläne in der gleichen Ressourcengruppe und geografischen Region befinden.

Navigieren Sie zu der App, die Sie in einen anderen Plan verschieben möchten. Suchen Sie im Menü **Einstellungen** nach der Option **App Service-Plan ändern**.

Das Auswahlelement **App Service-Plan** wird geöffnet. Hier können Sie entweder einen vorhandenen Plan auswählen oder einen neuen Plan erstellen. Es werden nur gültige Pläne angezeigt (derselben Ressourcengruppe und geografischen Region).

![Auswahlelement für App Service-Pläne][change]

Beachten Sie, dass jeder Plan eine eigene Preisstufe aufweist. Wenn Sie beispielsweise eine Website aus der Preisstufe „Free“ in die Preisstufe „Standard“ verschieben, kann Ihre App alle Features und Ressourcen der Preisstufe „Standard“ nutzen.

## Klonen einer App in einen anderen App Service-Plan
Wenn Sie die App in eine andere Region verschieben möchten, ist das Klonen der App eine Möglichkeit. Beim Klonen wird eine Kopie Ihrer App in einem neuen oder vorhandenen App Service-Plan oder einer App Service-Umgebung in einer beliebigen Region erstellt.

 ![Klonen einer App][appclone]

Die Option **App klonen** befindet sich im Menü **Extras**.

Beim Klonen gelten einige Einschränkungen, über die Sie sich unter [Klonen der Azure App Service-App über das Azure-Portal](../app-service-web/app-service-web-app-cloning-portal.md) informieren können.

## Skalieren eines App Service-Plans

Es gibt drei Möglichkeiten, einen Plan zu skalieren:

- **Ändern des Plantarifs:** Ein Plan der Preisstufe „Basic“ kann beispielsweise in einen Plan der Preisstufe „Standard“ oder „Premium“ geändert werden, sodass anschließend alle Apps, die diesem Plan zugeordnet sind, die von der neuen Preisstufe bereitgestellten Features nutzen können.
- **Ändern der Instanzgröße des Plans:** Ein Plan der Preisstufe „Basic“ mit kleinen Instanzen kann beispielsweise geändert werden, um große Instanzen zu verwenden. Alle Apps, die diesem Plan zugeordnet sind, können die zusätzlichen Arbeitsspeicher- und CPU-Ressourcen nutzen, die aufgrund der größeren Instanzen verfügbar sind.
- **Ändern der Instanzanzahl des Plans:** Ein horizontal auf drei Instanzen hochskalierter Standard-Plan kann beispielsweise auf zehn Instanzen skaliert werden. Ein Premium-Plan kann horizontal auf 20 Instanzen hochskaliert werden (sofern verfügbar). Alle Apps, die diesem Plan zugeordnet sind, können die zusätzlichen Arbeitsspeicher- und CPU-Ressourcen nutzen, die aufgrund der höheren Instanzanzahl verfügbar sind.

Sie können den Tarif und die Instanzgröße ändern, indem Sie für die App oder den App Service-Plan unter „Einstellungen“ auf die Option **Zentral hochskalieren** klicken. Änderungen gelten für den App Service-Plan und wirken sich auf alle Apps aus, die darunter gehostet werden.

 ![Festlegen von Werten zum zentralen Hochskalieren einer App][pricingtier]

## Zusammenfassung

App Service-Pläne stellen einen Satz an Funktionen und Kapazitäten dar, die Sie App-übergreifend gemeinsam nutzen können. Mit App Service-Plänen können Sie bestimmte Apps flexibel einer bestimmten Gruppe von Ressourcen zuordnen und die Auslastung Ihrer Azure-Ressourcen weiter optimieren. Dadurch können Sie beispielsweise in Ihrer Testumgebung Geld sparen, indem Sie einen Plan für mehrere Apps verwenden. Sie können außerdem den Durchsatz für Ihre Produktionsumgebung maximieren, indem Sie sie über mehrere Regionen und Pläne skalieren.

## Änderungen

* Informationen zu den Änderungen zwischen Websites und App Service finden Sie unter [Azure App Service und vorhandene Azure-Dienste](http://go.microsoft.com/fwlink/?LinkId=529714).

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
[appclone]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/app-clone.png

<!---HONumber=AcomDC_0817_2016-->