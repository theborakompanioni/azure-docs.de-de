<properties
	pageTitle="Erste Schritte mit Azure App Service und ASP.NET-Web-API 2 | Microsoft Azure"
	description="Erfahren Sie, wie Sie ein ASP.NET-Web-API 2-Projekt in Visual Studio erstellen und in einer neuen API-App in Azure App Service bereitstellen."
	services="app-service\api"
	documentationCenter=".net"
	authors="tdykstra"
	manager="wpickett"
	editor=""/>

<tags
	ms.service="app-service-api"
	ms.workload="web"
	ms.tgt_pltfrm="na"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="04/13/2016"
	ms.author="tdykstra"/>

# Erste Schritte mit Azure App Service und ASP.NET-Web-API 2 mit Visual Studio

## Übersicht

In diesem Tutorial wird erläutert, wie Sie mit Visual Studio 2015 eine ASP.NET-Web-API 2-Anwendung in einer [API-App](app-service-api-apps-why-best-platform.md) in Azure App Service bereitstellen. Sie erstellen ein Visual Studio-Projekt und stellen es dann für eine API-App bereit. Dies ist im Diagramm dargestellt.

![Visual Studio – Diagramm zur Erstellung und Bereitstellung](./media/app-service-api-dotnet-get-started-template/Create_App.png)

Das Tutorial geht davon aus, dass Sie ein ASP.NET-Entwickler sind, der noch keine Erfahrung mit der Verwendung von Azure hat. Nach Abschluss des Tutorials verfügen Sie über eine einfache Web-API, die in der Cloud ausgeführt wird.

Sie lernen Folgendes:

* Erstellen einer neuen App Service-API-App beim Erstellen eines neuen Web-API 2-Projekts in Visual Studio
* Bereitstellen eines Web-API 2-Projekts für eine App Service-API-App mit Visual Studio
* Verwenden des [Azure-Portals](/features/azure-portal/) zum Überwachen und Verwalten der API-App

Am Ende des Tutorials werden im Abschnitt [Problembehandlung](#troubleshooting) Hinweise dazu gegeben, was zu tun ist, wenn etwas nicht funktioniert. Der Abschnitt [Nächste Schritte](#next-steps) enthält Links zu anderen Tutorials, in denen die Verwendung von Azure App Service ausführlicher behandelt wird.

## Voraussetzungen

### Azure-Konto

Sie benötigen ein Azure-Konto, um das Tutorial durchführen zu können. Sie können:

* [Kostenloses Anlegen eines Azure-Kontos](/pricing/free-trial/?WT.mc_id=A261C142F). Sie erhalten ein Guthaben, mit dem Sie andere kostenpflichtige Azure-Dienste ausprobieren können. Auch nachdem Sie das Guthaben aufgebraucht haben, können Sie das Konto behalten und kostenlose Azure-Dienste und -Features nutzen.
* [Aktivieren Sie Visual Studio-Abonnementvorteile](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). Ihr MSDN-Abonnement beinhaltet ein monatliches Guthaben, das Sie für zahlungspflichtige Azure-Dienste verwenden können.

Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, besuchen Sie [Azure App Service-App erstellen](http://go.microsoft.com/fwlink/?LinkId=523751). Dort können Sie direkt eine kurzzeitige Start-App in App Service erstellen – ohne Kreditkarte und weitere Verpflichtungen.

### <a name="setupdevenv"></a>Visual Studio 2015 mit dem Azure SDK für .NET

Das Tutorial gilt für Visual Studio 2015 mit dem [Azure SDK für .NET](../dotnet-sdk.md) 2.8.2 oder höher. [Laden Sie hier das aktuelle Azure SDK für Visual Studio 2015 herunter](http://go.microsoft.com/fwlink/?linkid=518003). Visual Studio 2015 wird automatisch mit dem SDK installiert, wenn das Programm noch nicht vorhanden ist.

Wenn Sie Visual Studio 2013 besitzen, [laden Sie das aktuelle Azure SDK für Visual Studio 2013 herunter](http://go.microsoft.com/fwlink/?LinkID=324322). Einige Bildschirme unterscheiden sich unter Umständen von den Abbildungen.

>[AZURE.NOTE] Abhängig von der Anzahl an bereits bestehenden SDK-Abhängigkeiten auf Ihrem Computer kann der Installationsvorgang des SDK von mehreren Minuten bis hin zu einer halben Stunde oder länger dauern.

### ASP.NET-Web-API 2

In diesem Tutorial wird die Verwendung der ASP.NET-Web-API 2 mit Azure App Service beschrieben. Die Entwicklung einer ASP.NET-Web-API wird hier nicht behandelt. Eine Einführung zur ASP.NET-Web-API 2 finden Sie unter [Erste Schritte mit der ASP.NET-Web-API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) auf der [ASP.NET](http://asp.net/)-Website.

## Erstellen eines Projekts und einer API-App in Azure App Service

Der erste Schritt besteht darin, ein ASP.NET-Web-API 2-Projekt in Visual Studio und eine API-App in Azure App Service zu erstellen. Wenn Sie damit fertig sind, stellen Sie das Projekt für die API-App bereit, um die Web-API im Internet verfügbar zu machen.

1. Öffnen Sie Visual Studio 2015.

2. Klicken Sie auf **Datei > Neu > Projekt**.

3. Klicken Sie im Dialogfeld **Neues Projekt** auf **Visual C# > Web > ASP.NET-Webanwendung**. (Bei Bedarf können Sie auch **Visual Basic** verwenden.)

3. Stellen Sie sicher, dass als Zielframework **.NET Framework 4.5.2** ausgewählt ist.

4.  [Azure Application Insights](../application-insights/app-insights-overview.md) überwacht die Verfügbarkeit, Leistung und Nutzung Ihrer API-App. Das Kontrollkästchen **Application Insights zum Projekt hinzufügen** ist standardmäßig aktiviert, wenn Sie das erste Mal nach der Installation von Visual Studio ein Webprojekt erstellen. Deaktivieren Sie das Kontrollkästchen, wenn Sie Application Insights nicht ausprobieren möchten.

4. Nennen Sie die Anwendung **MyExample**.

5. Klicken Sie auf **OK**.

	![Dialogfeld "Neues Projekt"](./media/app-service-api-dotnet-get-started-template/GS13newprojdb.png)

5. Wählen Sie im Dialogfeld **Neues ASP.NET-Projekt** die Vorlage **Azure API-App** aus.

5. Überprüfen Sie im Dialogfeld **Neues ASP.NET-Projekt** im Abschnitt **Microsoft Azure**, ob **In der Cloud hosten** und in der Dropdownliste die Option **App Service** ausgewählt ist.

	![Dialogfeld "Neues ASP.NET-Projekt"](./media/app-service-api-dotnet-get-started-template/GS13newaspnetprojdb.png)

	Mit diesen Einstellungen wird Visual Studio angewiesen, eine Azure-API-App für das Webprojekt zu erstellen.

6. Klicken Sie auf **OK**.

5. Falls Sie sich noch nicht bei Azure angemeldet haben, werden Sie von Visual Studio dazu aufgefordert, dies nachzuholen. Melden Sie sich mit der ID und dem Kennwort des Kontos an, mit dem Sie Ihr Azure-Abonnement verwalten.

	Wenn Sie angemeldet sind, werden Sie im Dialogfeld **App Service erstellen** gefragt, welche Ressourcen Sie erstellen möchten.

	![Bei Azure angemeldet](./media/app-service-api-dotnet-get-started-template/configuresitesettings.png)

3. Geben Sie im Dialogfeld **App Service erstellen** einen **API-App-Namen** ein, der in der Domäne *azurewebsites.net* eindeutig ist. Beispielsweise können Sie sie "MyExample" nennen und Zahlen anhängen, damit der Name eindeutig ist, z. B. "MyExample810". Wenn ein Standardname für Sie erstellt wird, ist dieser eindeutig, und Sie können ihn verwenden.

	Wenn eine andere Person den eingegebenen Namen bereits verwendet hat, wird rechts ein rotes Ausrufungszeichen anstelle eines grünen Häkchens angezeigt, und Sie müssen einen anderen Namen eingeben.

	Die Basis-URL für Ihre Web-API ist dieser Name mit dem Zusatz *.azurewebsites.net* (wie neben dem Textfeld **Web-App-Name** angezeigt). Wenn der Name beispielsweise `MyExample810` lautet, ist die URL `myexample810.azurewebsites.net`.

6. Klicken Sie neben der Dropdownliste **Ressourcengruppe** auf **Neu**, und geben Sie dann „MyExample“ oder einen Namen Ihrer Wahl ein.

	In diesem Kombinationsfeld können Sie eine vorhandene Ressourcengruppe auswählen oder eine neue Ressourcengruppe erstellen. Geben Sie hierfür einen Namen ein, der sich von einer vorhandenen Ressourcengruppe in Ihrem Abonnement unterscheidet.

	Eine Ressourcengruppe ist eine Sammlung mit Azure-Ressourcen, z.B. API-Apps, Datenbanken und VMs. Für dieses Tutorial ist es im Allgemeinen besser, eine neue Ressourcengruppe zu erstellen. Es ist dann einfacher, alle Azure-Ressourcen, die Sie für das Tutorial erstellen, in einem Schritt zu löschen. Weitere Informationen finden Sie unter [Übersicht über den Azure Resource Manager](../resource-group-overview.md).

4. Klicken Sie neben der Dropdownliste **App Service-Plan** auf die Schaltfläche **Neu**.

	![Dialogfeld „App Service erstellen“](./media/app-service-api-dotnet-get-started-template/createas.png)

	Der Screenshot enthält Beispielwerte für die Optionen **Web-App-Name**, **Abonnement** und **Ressourcengruppe**. Ihre Werte werden sich von diesen Werten unterscheiden.

	In den folgenden Schritten erstellen Sie einen App Service-Plan für die neue Ressourcengruppe. Mit einem App Service-Plan werden die Computeressourcen angegeben, auf denen Ihre API-App ausgeführt wird. Wenn Sie beispielsweise den Free-Tarif wählen, wird Ihre API-App auf freigegebenen VMs ausgeführt. Für einige kostenpflichtige Tarife wird sie dagegen auf dedizierten VMs ausgeführt. Weitere Informationen finden Sie unter [App Service-Pläne – Übersicht](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

5. Geben Sie im Dialogfeld **App Service-Plan konfigurieren** den Namen „MyExamplePlan“ oder einen anderen Namen Ihrer Wahl ein.

5. Wählen Sie in der Dropdownliste **Standort** den Standort aus, der Ihnen am nächsten liegt.

	Mit dieser Einstellung wird das Azure-Rechenzentrum angegeben, in dem Ihre App ausgeführt wird. In diesem Lernprogramm können Sie ohne spürbaren Unterschied eine beliebige Region auswählen. Für eine Produktions-App sollte sich der Server aber so nah wie möglich bei den Clients befinden, mit denen darauf zugegriffen wird, um die [Latenz](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090) zu minimieren.

5. Klicken Sie in der Dropdownliste **Größe** auf **Free**.

	Für dieses Tutorial bietet der Tarif „Free“ eine ausreichende Leistung.

6. Klicken Sie im Dialogfeld **App Service-Plan konfigurieren** auf **OK**.

	![Dialogfeld „App Service konfigurieren“](./media/app-service-api-dotnet-get-started-template/configasp.png)

7. Klicken Sie im Dialogfeld **App Service erstellen** auf **Erstellen**.

	![Dialogfeld „App Service erstellen“](./media/app-service-api-dotnet-get-started-template/clickcreate.png)

	Visual Studio erstellt das Webprojekt und die API-App innerhalb kurzer Zeit, üblicherweise in weniger als einer Minute.

	Im Fenster **Projektmappen-Explorer** werden die Dateien und Ordner in dem neuen Projekt angezeigt.

	![Projektmappen-Explorer](./media/app-service-api-dotnet-get-started-template/solutionexplorer.png)

	Im Fenster **Aktivität von Azure App Service** wird angegeben, dass die API-App erstellt wurde. (In der Nachricht wird sie ggf. als Web-App bezeichnet.)

	![Im Fenster „Aktivität von Azure App Service“ erstellte API-App](./media/app-service-api-dotnet-get-started-template/GS13sitecreated1.png)

	Außerdem können Sie die API-App im Fenster **Cloud-Explorer** von Visual Studio anzeigen.

	![Im Cloud-Explorer erstellte API-App](./media/app-service-api-dotnet-get-started-template/siteinse.png)
	
	In diesem Fenster können Sie viele Azure-Ressourcen anzeigen und verwalten. Es kann sein, dass im Fenster **Cloud-Explorer** andere Ressourcentypen als in diesem Beispiel angezeigt werden. Klicken Sie mit der rechten Maustaste auf eine Ressource, z.B. Ihre API-App, um die verfügbaren Optionen für die Verwaltung anzuzeigen.

## Bereitstellen des Visual Studio-Projekts für die Azure-API-App

In diesem Abschnitt stellen Sie das Webprojekt in der API-App bereit (siehe Schritt 2 in der Abbildung).

![Visual Studio – Diagramm zur Erstellung und Bereitstellung](./media/app-service-api-dotnet-get-started-template/Create_App.png)

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen** aus.

	![„Veröffentlichen“ im Visual Studio-Menü auswählen](./media/app-service-api-dotnet-get-started-template/choosepublish.png)

	Wenige Sekunden später wird der Assistent **Web veröffentlichen** geöffnet. Im Assistenten wird ein *Veröffentlichungsprofil* mit Einstellungen für die Bereitstellung des Projekts in der neuen API-App angezeigt. Wenn die Bereitstellung in einer anderen API-App erfolgen soll, können Sie auf die Registerkarte **Profil** klicken, um ein anderes Profil zu erstellen. In diesem Tutorial übernehmen Sie die Einstellungen für die Bereitstellung der zuvor erstellten API-App.

	Das Veröffentlichungsprofil enthält einen Benutzernamen und ein Kennwort für die Bereitstellung. Diese Anmeldeinformationen wurden für Sie generiert, und Sie müssen sie nicht eingeben oder ändern. Das Kennwort wird in einer ausgeblendeten benutzerspezifischen Datei im Ordner `Properties\PublishProfiles` verschlüsselt.

8. Klicken Sie auf der Registerkarte **Verbindung** des Assistenten **Web veröffentlichen** auf **Weiter**.

	![Im Assistenten „Web veröffentlichen“ auf der Registerkarte „Verbindung“ auf „Weiter“ klicken](./media/app-service-api-dotnet-get-started-template/GS13ValidateConnection.png)

	Die nächste Registerkarte ist die Registerkarte **Einstellungen** (unten dargestellt). Hier können Sie die Buildkonfiguration ändern, um einen Debugbuild für das [Remotedebuggen](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug) bereitzustellen. Die Registerkarte enthält außerdem mehrere **Dateiveröffentlichungsoptionen**:

	* Nicht übereinstimmende Dateien im Ziel entfernen
	* Während der Veröffentlichung vorkompilieren
	* Dateien aus dem App\_Data-Ordner ausschließen

	Für dieses Tutorial benötigen Sie diese Optionen nicht. Ausführliche Beschreibungen der Funktionsweise dieser Optionen finden Sie unter [Gewusst wie: Bereitstellen eines Webanwendungsprojekts mit der One-Click-Veröffentlichung in Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx).

10. Klicken Sie auf der Registerkarte **Einstellungen** auf **Weiter**.

	![Registerkarte „Einstellungen“ im Assistenten „Web veröffentlichen“](./media/app-service-api-dotnet-get-started-template/GS13SettingsTab.png)

	Die nächste Registerkarte ist die Registerkarte **Vorschau** (unten dargestellt). Hier können Sie sehen, welche Dateien aus Ihrem Projekt in die API-App kopiert werden. Wenn Sie ein Projekt für eine API-App bereitstellen, für die Sie bereits eine Bereitstellung durchgeführt haben, werden nur geänderte Dateien kopiert. Wenn Sie eine Liste mit den kopierten Dateien anzeigen möchten, können Sie auf die Schaltfläche **Vorschau starten** klicken.

11. Klicken Sie auf der Registerkarte **Vorschau** auf **Veröffentlichen**.

	![Registerkarte „Vorschau“ im Assistenten „Web veröffentlichen“](./media/app-service-api-dotnet-get-started-template/GS13previewoutput.png)

	Wenn Sie auf **Veröffentlichen** klicken, werden die Dateien von Visual Studio auf den Azure-Server kopiert. Dies kann ein oder zwei Minuten dauern.

	In den Fenstern **Ausgabe** und **Azure App Service-Aktivität** wird angezeigt, welche Bereitstellungsaktionen ausgeführt wurden, und es wird die erfolgreiche Durchführung der Bereitstellung gemeldet.

	![Meldung zur erfolgreichen Bereitstellung im Visual Studio-Ausgabefenster](./media/app-service-api-dotnet-get-started-template/PublishOutput.png)

	Nach der erfolgreichen Bereitstellung wird die Basis-URL der bereitgestellten API-App automatisch im Standardbrowser geöffnet. Der Browser zeigt eine Seite mit einer Meldung der Art „Die Web-App wurde erfolgreich erstellt“ an.

	![Meldung zur erfolgreichen Bereitstellung im Visual Studio-Ausgabefenster](./media/app-service-api-dotnet-get-started-template/successfullycreated.png)

> [AZURE.TIP] Um eine schnelle Bereitstellung zu ermöglichen, können Sie die Symbolleiste **Webveröffentlichung mit einem Klick** aktivieren. Klicken Sie auf **Ansicht > Symbolleisten**, und wählen Sie dann **Webveröffentlichung mit einem Klick** aus. Auf der Symbolleiste können Sie ein Profil auswählen, auf eine Schaltfläche zum Veröffentlichen klicken oder auf eine Schaltfläche klicken, um den Assistenten **Web veröffentlichen** zu öffnen. ![Symbolleiste "Webveröffentlichung mit einem Klick"](./media/app-service-api-dotnet-get-started-template/weboneclickpublish.png)

## Testen der bereitgestellten Web-API

1. Senden Sie eine HTTP GET-Anforderung an die URL *{Name Ihrer API-App}.azurewebsites.net/api/values*, indem Sie das von Ihnen bevorzugte HTTP-Clienttool verwenden.

	Mit der Web-API-Projektvorlage wird ein `Values`-Controller definiert, mit dem ein Array mit zwei Zeichenfolgen im JSON-Format für eine GET-Anforderung zurückgegeben wird. Die folgende Abbildung zeigt eine von [Postman](http://www.getpostman.com/) gesendete Anforderung, bei der der JSON-Code im Text der Antwort zurückgegeben wird.

	![Meldung zur erfolgreichen Bereitstellung im Visual Studio-Ausgabefenster](./media/app-service-api-dotnet-get-started-template/postman.png)

2. Sie können jetzt Änderungen am Code vornehmen und das Projekt genauso wie bei der ersten Bereitstellung bereitstellen. Ihre Änderungen werden in Azure dann nach wenigen Sekunden wirksam.

## <a id="portal"></a> Optional: Überwachen und Verwalten der API-App im Azure-Portal

Das [Azure-Portal](/services/management-portal/) ist eine Weboberfläche, die das Verwalten und Überwachen Ihrer Azure-Dienste ermöglicht, z.B. der soeben erstellten API-App. In diesem Abschnitt des Lernprogramms erfahren Sie, was Sie im Portal durchführen können.

1. Rufen Sie in Ihrem Browser [https://portal.azure.com](https://portal.azure.com) auf, und melden Sie sich mit den Azure-Anmeldeinformationen an, die Sie für die Verwaltung Ihres Azure-Kontos verwenden.
	
2. Klicken Sie auf **App Services** und dann auf den Namen der API-App.

	![App Services im Azure-Portal](./media/app-service-api-dotnet-get-started-template/selinportal.png)

	Auf dem Blatt **API-App** werden eine Übersicht über die Einstellungen und eine Nutzungsstatistik für Ihre API-App angezeigt. (Fenster, die im Portal auf der rechten Seite geöffnet werden, werden als *Blätter* bezeichnet.)

	![API-App-Blatt im Azure-Portal](./media/app-service-api-dotnet-get-started-template/portaldashboard.png)

	In Ihrer API-App gab es bisher kaum Datenverkehr, sodass in der Übersicht unter Umständen nichts angezeigt wird. Wenn Sie weitere Anforderungen an die Web-API senden und anschließend die Portalseite aktualisieren, werden einige Statistiken angezeigt.

3. Auf dem Blatt **Einstellungen** werden weitere Optionen zum Konfigurieren der API-App angezeigt.

	![Blatt „Einstellungen“ im Azure-Portal](./media/app-service-api-dotnet-get-started-template/portalconfigure1.png)

	Der Abschnitt **API** enthält Links zu Blättern zum Konfigurieren eines API-Metadatenendpunkts und von CORS. Unter [Nächste Schritte](#next-steps) finden Sie Links zu Tutorials, in denen diese Features vorgestellt werden.

	Beachten Sie den Link **Anmeldeinformationen für Bereitstellung** im Abschnitt **Veröffentlichung**. Hier können Sie einen benutzerdefinierten Benutzernamen mit dazugehörigem Kennwort für die Bereitstellung erstellen. Klicken Sie oben im Blatt auf die Schaltfläche **Speichern**, um Ihre Änderung abzusenden. Wenn Sie einen neuen Benutzernamen mit dazugehörigem Kennwort erstellen, müssen Sie in Ihrem Webprojekt im Assistenten **Web veröffentlichen** auf der Registerkarte **Verbindung** die gleichen Werte eingeben.
	
	Der Screenshot zeigt nur eine Teilansicht des Blatts **Einstellungen**. Dieses Blatt enthält noch mehr Abschnitte, die hier nicht angezeigt werden.

Dies sind nur einige Funktionen des Portals. Sie können neue API-Apps erstellen, vorhandene API-Apps löschen, API-Apps anhalten und neu starten und andere Azure-Dienste verwalten, z.B. Datenbanken und virtuelle Computer.

## Optional: Löschen der Azure-API-App

Wenn Sie die für dieses Tutorial erstellte API-App nicht mehr benötigen, können Sie sie löschen.

Eine einfache Möglichkeit, die API-App zu löschen, ist das Klicken auf die Schaltfläche **Löschen** im Azure-Portal oben auf dem Blatt **API-App**. Eine bessere Option ist das Löschen der Ressourcengruppe, die Sie für die API-App erstellt haben. In diesem Tutorial enthält die Ressourcengruppe nur die API-App, aber normalerweise enthält eine Ressourcengruppe eine Sammlung mit verwandten Ressourcen. Für die API-App kann beispielsweise eine Datenbank oder ein Azure-Speicherkonto als Ressource verwendet werden, die nicht mehr erforderlich ist, wenn Sie die API-App löschen. Beim Löschen einer Ressourcengruppe wird ihr gesamter Inhalt gelöscht. Führen Sie die folgenden Schritte aus, um eine Ressourcengruppe mit dem Azure-Portal zu löschen.

1. Wechseln Sie zur Startseite des [Azure-Portals](https://portal.azure.com).

2. Klicken Sie auf **Ressourcengruppen**.

3. Klicken Sie in der Liste mit den Ressourcengruppen auf die Ressourcengruppe, die Sie löschen möchten.

	Das Blatt **Ressourcengruppe** wird angezeigt und enthält eine Liste mit den darin enthaltenen Ressourcen.

4. Klicken Sie im Blatt **Ressourcengruppe** auf **Löschen**.

	![Ressourcengruppe im Azure-Portal löschen](./media/app-service-api-dotnet-get-started-template/delresgrp.png)

## Problembehandlung

Wenn Sie beim Durcharbeiten dieses Tutorials auf ein Problem stoßen, sollten Sie sich vergewissern, dass Sie die aktuelle Version des Azure SDK für .NET verwenden. Die einfachste Möglichkeit ist hierbei das [Herunterladen des Azure SDK für Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003). Wenn Sie die aktuelle Version installiert haben, werden Sie vom Webplattform-Installer darüber informiert, dass keine Installation erforderlich ist.

Falls Sie sich in einem Unternehmensnetzwerk befinden und versuchen, die Bereitstellung für Azure App Service über eine Firewall durchzuführen, sollten Sie sicherstellen, dass die Ports 443 und 8172 für Web Deploy geöffnet sind. Wenn Sie diese Ports nicht öffnen können, helfen Ihnen die anderen Bereitstellungsoptionen unten im Abschnitt „Nächste Schritte“ weiter.

Nachdem Ihre ASP.NET-API-App in Azure App Service ausgeführt wird, ist es ratsam, sich weiter über Visual Studio-Features zu informieren, mit denen die Problembehandlung vereinfacht werden kann. Informationen zu Protokollierung, Remotedebuggen und weiteren Themen finden Sie unter [Problembehandlung von Azure-API-Apps in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie eine einfache Web-API erstellt und in einer API-App in Azure App Service bereitgestellt wird. Eine Einführung in App Service-Features, die Ihnen die Entwicklung und Nutzung von Web-APIs erleichtern, finden Sie in der Tutorial-Reihe, die mit [Erste Schritte mit API-Apps und ASP.NET](app-service-api-dotnet-get-started.md) beginnt.

<!---HONumber=AcomDC_0504_2016-->