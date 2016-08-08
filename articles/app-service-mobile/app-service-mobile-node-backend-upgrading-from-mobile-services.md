<properties
	pageTitle="Upgrade von Mobile Services auf Azure App Service – Node.js"
	description="Erfahren Sie, wie einfach Sie Ihre Mobile Services-Anwendung auf eine mobile App Service-App aktualisieren können."
	services="app-service\mobile"
	documentationCenter=""
	authors="adrianhall"
	manager="ggailey"
	editor=""/>

<tags
	ms.service="app-service-mobile"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile"
	ms.devlang="node"
	ms.topic="article"
	ms.date="05/05/2016"
	ms.author="adrianha"/>

# Aktualisieren eines vorhandenen Node.js-Azure Mobile Service auf App Service

Mobile App Service-Apps sind eine neue Methode zum Erstellen mobiler Anwendungen mit Microsoft Azure. Weitere Informationen finden Sie unter [Was sind Mobile Apps?].

In diesem Thema wird beschrieben, wie Sie eine vorhandene Node.js-Back-End-Anwendung von Azure Mobile Services auf das neue Mobile App Service-Apps upgraden. Während Sie dieses Upgrade ausführen, können Sie Ihre vorhandene Mobile Services-Anwendung weiter betreiben. Wenn Sie eine Node.js-Back-End-Anwendung aktualisieren müssen, helfen Ihnen die Informationen unter [Aktualisieren des vorhandenen .NET-Azure Mobile Service auf App Service](./app-service-mobile-net-upgrading-from-mobile-services.md) weiter.

Wenn ein Mobil-Back-End auf Azure App Service aktualisiert wird, hat dieses Zugriff auf alle App Service-Features, und die Abrechnung erfolgt gemäß [App Service-Preisen], nicht gemäß den Mobile Services-Preisen.

## Vergleich von Migration und Upgrade

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] Es wird empfohlen, vor einem Upgrade eine [Migration auszuführen](app-service-mobile-migrating-from-mobile-services.md). Auf diese Weise können Sie beide Versionen der Anwendung in den gleichen App Service-Plan übernehmen, ohne dass zusätzliche Kosten anfallen.

### Verbesserungen im Mobile Apps-Server-SDK für Node.js

Bei einem Upgrade auf das neue [Mobile Apps-SDK](https://www.npmjs.com/package/azure-mobile-apps) profitieren Sie von vielen Verbesserungen:

- Das auf dem [Express-Framework](http://expressjs.com/en/index.html) basierende neue Node-SDK ist besonders schlank und so konzipiert, dass es mit künftigen neuen Node-Versionen Schritt hält. Sie können das Anwendungsverhalten mit der Express-Middleware anpassen.

- Beträchtliche Leistungssteigerungen im Vergleich zum Mobile Services SDK.

- Sie können nun eine Website zusammen mit dem Mobil-Back-End hosten. Zudem ist es einfach, das Azure Mobile Apps SDK einer beliebigen vorhandenen v4-Anwendung hinzuzufügen.

- Das Mobile Apps SDK ist für die plattformübergreifende und lokale Entwicklung ausgelegt. Es kann lokal auf Windows-, Linux- und OS X-Plattformen ausgeführt und zur Entwicklung genutzt werden. Es ist nun einfach, häufig verwendete Node-Entwicklungstechniken zu verwenden, z. B. das Ausführen von [Mocha](https://mochajs.org/)-Tests vor der Bereitstellung.

## <a name="overview"></a>Grundlegende Übersicht über den Upgradevorgang

Zur Unterstützung von Upgrades eines Node.js-Back-Ends wurde für Azure App Service ein Kompatibilitätspaket bereitgestellt. Nach dem Upgrade verfügen Sie über eine neue Website, die für eine neue App Service-Website bereitgestellt werden kann.

Die Mobile Services-Client-SDKs sind **nicht** mit dem neuen Mobile Apps-Server-SDK kompatibel. Um die Kontinuität von Diensten für Ihre App zu gewährleisten, sollten Sie keine Änderungen auf einer Website veröffentlichen, die derzeit veröffentlichte Clients bereitstellt. Stattdessen sollten Sie eine neue mobile App erstellen, die als Duplikat dient. Sie können diese Anwendung in den gleichen App Service-Plan aufnehmen, um zu vermeiden, dass zusätzliche Kosten anfallen.

Anschließend verfügen Sie über zwei Versionen der Anwendung: eine unveränderte Version, die veröffentlichte praktisch genutzte Apps bedient, und eine zweite Version, die Sie upgraden und als Ziel für eine neue Clientversion verwenden. Sie können den eigenen Code im eigenen Tempo verschieben und testen, aber sie sollten sicherstellen, dass alle Fehlerbehebungen, die Sie vornehmen, auf beide Versionen angewendet werden. Wenn Sie das Gefühl haben, dass die gewünschte Anzahl von praktisch genutzten Client-Apps auf die neueste Version aktualisiert wurde, können Sie die ursprüngliche migrierte App je nach Wunsch löschen. Solange die zweite Version im gleichen App Service-Plan gehostet wird wie Ihre vorhandene mobile App, fallen dafür keine zusätzlichen Kosten an.

Der vollständige Überblick über den Upgradevorgang sieht wie folgt aus:

1. Laden Sie Ihren vorhandenen (migrierten) Azure Mobile Service herunter.
2. Konvertieren Sie das Projekt mit dem Kompatibilitätspaket in eine Azure Mobile App.
3. Beseitigen Sie die Unterschiede (z.B. Authentifizierungseinstellungen).
4. Stellen Sie das konvertierte Azure Mobile App-Projekt unter einer neuen App Service-Instanz bereit.
4. Veröffentlichen Sie eine neue Version Ihrer Clientanwendung, die die neue mobile App nutzt.
5. (Optional) Löschen Sie die ursprünglich migrierte Mobile Service-App.

Löschung kann auftreten, wenn Sie keinen Datenverkehr in Ihrem ursprünglich migrierten Mobile Service feststellen.

## <a name="install-npm-package"></a> Installation der Voraussetzungen

Sie müssen [Node] auf Ihrem lokalen Computer installieren. Außerdem sollten Sie das Kompatibilitätspaket installieren. Nach der Installation von Node können Sie an einer neuen Befehlseingabeaufforderung oder PowerShell-Eingabeaufforderung den folgenden Befehl ausführen:

```npm i -g azure-mobile-apps-compatibility```

## <a name="obtain-ams-scripts"></a> Beschaffen von Azure Mobile Services-Skripts

- Melden Sie sich beim [Azure-Portal] an.
- Sie können mit **Alle Ressourcen** oder **App Services** nach Ihrer Mobile Services-Website suchen.
- Klicken Sie auf der Website auf **Tools** > **Kudu** > **Gehe zu**, um die Kudu-Website zu öffnen.
- Klicken Sie auf **Debugkonsole** > **PowerShell**, um die Debugkonsole zu öffnen.
- Navigieren Sie zu `site/wwwroot/App_Data/config`, indem Sie nacheinander auf die einzelnen Verzeichnisse klicken.
- Klicken Sie auf das Downloadsymbol neben dem Verzeichnis `scripts`.

Die Skripts werden im ZIP-Format heruntergeladen. Erstellen Sie auf dem lokalen Computer ein neues Verzeichnis, und entpacken Sie die Datei `scripts.ZIP` im Verzeichnis. Das Verzeichnis `scripts` wird erstellt.

## <a name="scaffold-app"></a> Erstellen des Gerüsts für das neue Azure Mobile Apps-Back-End

Führen Sie den folgenden Befehl in dem Verzeichnis aus, in dem das Verzeichnis „scripts“ enthalten ist:

```scaffold-mobile-app scripts out```

Ein mit einem Gerüst versehenes Azure Mobile Apps-Back-End wird im Verzeichnis `out` erstellt. Es ist zwar nicht zwingend erforderlich, aber trotzdem ratsam, das Verzeichnis `out` in ein Quellcoderepository Ihrer Wahl einzuchecken.

## <a name="deploy-ama-app"></a> Bereitstellen des Azure Mobile Apps-Back-Ends

Während der Bereitstellung müssen Sie die folgenden Schritte ausführen:

1. Erstellen Sie im [Azure-Portal] eine neue mobile App.
2. Führen Sie das Skript `createViews.sql` in Ihrer verbundenen Datenbank aus.
3. Verknüpfen Sie die Datenbank, die mit dem Mobile Service verknüpft ist, mit Ihrer neuen App Service-Instanz.
4. Verknüpfen Sie alle anderen Ressourcen (z.B. Notification Hubs) mit der neuen App Service-Instanz.
5. Stellen Sie den generierten Code auf Ihrer neuen Website bereit.

### Erstellen einer neuen mobilen App

1. Melden Sie sich beim [Azure-Portal] an.

2. Klicken Sie auf **+ NEU** > **Web und mobil** > **Mobile App**, und vergeben Sie dann einen Namen für Ihr Mobile App-Back-End.

3. Wählen Sie für die **Ressourcengruppe** eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue (verwenden Sie dabei den gleichen Namen wie für die App).
 
	Sie können einen anderen vorhandenen App Service-Plan auswählen oder einen neuen Plan erstellen. Weitere Informationen zu App Service-Plänen sowie zum Erstellen eines neuen Plans in einem anderen Tarif und am gewünschten Standort finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

4. Für den **App Service-Plan** ist der Standardplan (im [Standard-Tarif](https://azure.microsoft.com/pricing/details/app-service/)) ausgewählt. Sie können auch einen anderen Plan auswählen oder [einen neuen Plan erstellen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). Die Einstellungen für den App Service-Plan bestimmen [den Standort, die Funktionen, die Kosten und die Computeressourcen](https://azure.microsoft.com/pricing/details/app-service/) Ihrer App.

	Nachdem Sie den Plan festgelegt haben, klicken Sie auf **Erstellen**. Dadurch wird das Back-End für mobile Apps erstellt.


### Ausführen von „CreateViews.SQL“

Die mit dem Gerüst versehene App enthält eine Datei mit dem Namen `createViews.sql`. Dieses Skript muss für die Zieldatenbank ausgeführt werden. Die Verbindungszeichenfolge für die Zieldatenbank finden Sie im migrierten Mobile Service auf dem Blatt **Einstellungen** unter **Verbindungszeichenfolgen**. Sie lautet `MS_TableConnectionString`.

Sie können dieses Skript in SQL Server Management Studio oder Visual Studio ausführen.

### Verknüpfen der Datenbank mit der App Service-Instanz

Verknüpfen Sie die vorhandene Datenbank mit der App Service-Instanz:

- Öffnen Sie im [Azure-Portal] Ihre App Service-Instanz.
- Wählen Sie **Alle Einstellungen** > **Datenverbindungen**.
- Klicken Sie auf **+ Hinzufügen**.
- Wählen Sie in der Dropdownliste die Option **SQL-Datenbank**.
- Wählen Sie unter **SQL-Datenbank** die vorhandene Datenbank aus, und klicken Sie dann auf **Auswählen**.
- Geben Sie unter **Verbindungszeichenfolge** den Benutzernamen und das Kennwort für die Datenbank ein, und klicken Sie dann auf **OK**.
- Klicken Sie auf dem Blatt **Datenverbindungen hinzufügen** auf **OK**.

Sie erhalten den Benutzernamen und das Kennwort, indem Sie die Verbindungszeichenfolge für die Zieldatenbank im migrierten Mobile Service anzeigen.


### Einrichten der Authentifizierung

Mit Azure Mobile Apps können Sie die Authentifizierung per Azure Active Directory, Facebook, Google, Microsoft und Twitter im Dienst konfigurieren. Die benutzerdefinierte Authentifizierung muss separat entwickelt werden. Weitere Informationen finden Sie in der Dokumentation zum Thema [Authentifizierungskonzepte] und [Authentifizierung – Schnellstart].

## <a name="updating-clients"></a>Aktualisieren von mobilen Clients

Sobald Sie über ein betriebsbereites Mobile App-Back-End verfügen, können Sie an einer neuen Version Ihrer Clientanwendung arbeiten, die dieses nutzt. Mobile Apps enthält außerdem eine neue Version der Client-SDKs. Wie bei dem oben beschriebenen Serverupgrade müssen Sie vor der Installation der Mobile Apps-Versionen alle Verweise auf die Mobile Services-SDKs entfernen.

Einer der wichtigsten Unterschiede zwischen den Versionen ist, dass die Konstruktoren keinen Anwendungsschlüssel mehr benötigen. Sie brauchen jetzt nur noch die URL der mobilen App zu übergeben. Bei den .NET-Clients lautet der `MobileServiceClient`-Konstruktor z. B. nun:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

Informationen zum Installieren der neuen SDKs und zum Verwenden der neuen Struktur finden Sie unter den folgenden Links:

- [Android Version 2.2 oder höher](app-service-mobile-android-how-to-use-client-library.md)
- [iOS 3.0.0 oder höher](app-service-mobile-ios-how-to-use-client-library.md)
- [.NET (Windows/Xamarin) 2.0.0 oder höher](app-service-mobile-dotnet-how-to-use-client-library.md)
- [Apache Cordova Version 2.0 oder höher](app-service-mobile-cordova-how-to-use-client-library.md)

Wenn Ihre Anwendung Pushbenachrichtigungen verwendet, notieren Sie sich die speziellen Registrierungsanweisungen für jede Plattform, weil auch dort einige Änderungen vorgenommen wurden.

Wenn die neue Clientversion bereit ist, testen Sie sie mit dem aktualisierten Serverprojekt. Nachdem Sie sich vergewissert haben, dass sie funktioniert, können Sie eine neue Version der Anwendung für die Kunden freigeben. Nachdem Ihre Kunden diese Updates erhalten haben, können Sie die Mobile Services-Version Ihrer App schließlich löschen. An diesem Punkt haben Sie die vollständige Aktualisierung auf eine mobile App Service-App durchgeführt und dafür das aktuelle Mobile Apps-Server-SDK verwendet.

<!-- URLs. -->

[Azure portal]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[Was sind Mobile Apps?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /de-DE/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: https://www.npmjs.com/package/azure-mobile-apps
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /de-DE/documentation/services/scheduler/
[Web Job]: ../app-service-web/websites-webjobs-resources.md
[How to use the .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[App Service-Preisen]: https://azure.microsoft.com/de-DE/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Authentifizierungskonzepte]: ../app-service/app-service-authentication-overview.md
[Authentifizierung – Schnellstart]: app-service-mobile-auth.md

[Azure-Portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/de-DE/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/de-DE/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston

<!---HONumber=AcomDC_0727_2016-->