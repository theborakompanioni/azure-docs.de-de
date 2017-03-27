---
title: Erstellen einer Web-App mit Redis Cache | Microsoft Docs
description: Hier erfahren Sie, wie Sie eine Web-App mit Redis Cache erstellen.
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 454e23d7-a99b-4e6e-8dd7-156451d2da7c
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: hero-article
ms.date: 03/21/2017
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 1429bf0d06843da4743bd299e65ed2e818be199d
ms.openlocfilehash: 5e7520f8a023cd5feb8401483161e7296a413b02
ms.lasthandoff: 03/22/2017


---
# <a name="how-to-create-a-web-app-with-redis-cache"></a>Gewusst wie: Erstellen einer Web-App mit Redis Cache
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

In diesem Tutorial wird erläutert, wie Sie mithilfe von Visual Studio 2015 eine ASP.NET-Webanwendung erstellen und für eine Web-App in Azure App Service bereitstellen. Die Beispielanwendung zeigt eine Teamstatistik aus einer Datenbank an und veranschaulicht verschiedene Möglichkeiten zum Speichern und Abrufen von Daten aus dem Cache mithilfe von Azure Redis Cache. Nach Abschluss des Tutorials verfügen Sie über eine funktionsfähige Web-App, die Lese- und Schreibvorgänge in einer Datenbank durchführt, mit Azure Redis Cache optimiert ist und in Azure gehostet wird.

Sie lernen Folgendes:

* Erstellen einer ASP.NET MVC 5-Webanwendung in Visual Studio
* Zugreifen auf Daten aus einer Datenbank per Entity Framework
* Optimieren des Datendurchsatzes und Verringern der Datenbanklast durch Speichern und Abrufen von Daten mithilfe von Azure Redis Cache
* Abrufen der fünf besten Teams mithilfe eines sortierten Redis-Satzes
* Bereitstellen der Azure-Ressourcen für die Anwendung mithilfe einer Resource Manager-Vorlage
* Veröffentlichen der Anwendung für Azure mithilfe von Visual Studio

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie Folgendes:

* [Azure-Konto](#azure-account)
* [Visual Studio 2017 mit dem Azure SDK für .NET](#visual-studio-2017-with-the-azure-sdk-for-net)

### <a name="azure-account"></a>Azure-Konto
Sie benötigen ein Azure-Konto, um das Tutorial durchführen zu können. Sie können:

* [Kostenloses Anlegen eines Azure-Kontos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero). Sie erhalten ein Guthaben, mit dem Sie andere kostenpflichtige Azure-Dienste ausprobieren können. Auch nachdem Sie das Guthaben aufgebraucht haben, können Sie das Konto behalten und kostenlose Azure-Dienste und -Features nutzen.
* [Aktivieren Sie Visual Studio-Abonnementvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Ihr MSDN-Abonnement beinhaltet ein monatliches Guthaben, das Sie für zahlungspflichtige Azure-Dienste verwenden können.

### <a name="visual-studio-2017-with-the-azure-sdk-for-net"></a>Visual Studio 2017 mit dem Azure SDK für .NET
Das Tutorial wurde für Visual Studio 2017 mit dem [Azure SDK für .NET](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes#azuretools) geschrieben. Das Azure SDK 2.9.5 ist im Visual Studio-Installer enthalten.

Wenn Sie Visual Studio 2015 haben, können Sie das Tutorial mit dem [Azure-SDK für .NET](../dotnet-sdk.md) 2.8.2 oder höher durcharbeiten. Das aktuelle Azure SDK für Visual Studio 2015 können Sie [hier](http://go.microsoft.com/fwlink/?linkid=518003) herunterladen. Visual Studio&2015; wird automatisch mit dem SDK installiert, wenn das Programm noch nicht vorhanden ist. Einige Bildschirme unterscheiden sich unter Umständen von den Abbildungen in diesem Tutorial.

Bei Verwendung von Visual Studio 2013 können Sie [das aktuelle Azure SDK für Visual Studio 2013 herunterladen](http://go.microsoft.com/fwlink/?LinkID=324322). Einige Bildschirme unterscheiden sich unter Umständen von den Abbildungen in diesem Tutorial.

## <a name="create-the-visual-studio-project"></a>Erstellen des Visual Studio-Projekts
1. Öffnen Sie Visual Studio, und klicken Sie auf **Datei** > **Neu** > **Projekt**.
2. Erweitern Sie in der Liste **Vorlagen** den Knoten **Visual C#**, wählen Sie **Cloud** aus, und klicken Sie anschließend auf **ASP.NET-Webanwendung**. Vergewissern Sie sich, dass mindestens **.NET Framework 4.5.2** ausgewählt ist.  Geben Sie im Textfeld **Name** die Zeichenfolge **ContosoTeamStats** ein, und klicken Sie auf **OK**.
   
    ![Projekt erstellen][cache-create-project]
3. Wählen Sie als Projekttyp die Option **MVC** aus. 

    Stellen Sie sicher, dass für die Einstellungen unter **Authentifizierung** die Option **Keine Authentifizierung** angegeben ist. Je nach Ihrer Version von Visual Studio kann der Standardwert auch anders lauten. Um dies zu ändern, klicken Sie auf **Authentifizierung ändern** und wählen **Keine Authentifizierung**.

    Falls Sie für diesen Vorgang Visual Studio 2015 nutzen, sollten Sie das Kontrollkästchen **In der Cloud hosten** deaktivieren. Die Schritte zum [Bereitstellen der Azure-Ressourcen](#provision-the-azure-resources) und [Veröffentlichen der Anwendung für Azure](#publish-the-application-to-azure) werden im weiteren Verlauf des Tutorials ausgeführt. Ein Beispiel, in dem eine App Service-Web-App über Visual Studio mit aktiviertem Kontrollkästchen **In der Cloud hosten** bereitgestellt wird, finden Sie unter [Erste Schritte mit Web-Apps in Azure App Service mit ASP.NET und Visual Studio](../app-service-web/web-sites-dotnet-get-started.md).
   
    ![Projektvorlage auswählen][cache-select-template]
4. Klicken Sie auf **OK** , um das Projekt zu erstellen.

## <a name="create-the-aspnet-mvc-application"></a>Erstellen der ASP.NET MVC-Anwendung
In diesem Abschnitt des Tutorials wird die grundlegende Anwendung zum Lesen und Anzeigen der Teamstatistik aus einer Datenbank erstellt.

* [Hinzufügen des Entity Framework-NuGet-Pakets](#add-the-entity-framework-nuget-package)
* [Hinzufügen des Modells](#add-the-model)
* [Hinzufügen des Controllers](#add-the-controller)
* [Konfigurieren der Ansichten](#configure-the-views)

### <a name="add-the-entity-framework-nuget-package"></a>Hinzufügen des Entity Framework-NuGet-Pakets

1. Klicken Sie im Menü **Extras** auf **NuGet-Paket-Manager** > **Paket-Manager-Konsole**.
2. Führen Sie im Fenster `Package Manager Console` den folgenden Befehl aus:
    
    ```
    Install-Package EntityFramework
    ```

Weitere Informationen zu diesem Paket finden Sie auf der [EntityFramework](https://www.nuget.org/packages/EntityFramework/)-NuGet-Seite.

### <a name="add-the-model"></a>Hinzufügen des Modells
1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Modelle**, und wählen Sie **Hinzufügen** > **Klasse** aus. 
   
    ![Modell hinzufügen][cache-model-add-class]
2. Geben Sie als Klassenname die Zeichenfolge `Team` ein, und klicken Sie anschließend auf **Hinzufügen**.
   
    ![Modellklasse hinzufügen][cache-model-add-class-dialog]
3. Ersetzen Sie die Anweisungen vom Typ `using` (am Anfang der Datei `Team.cs`) durch folgende `using`-Anweisungen:

    ```c#
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Data.Entity.SqlServer;
    ```


1. Ersetzen Sie die Definition der `Team`-Klasse durch den folgenden Codeausschnitt. Dieser enthält eine aktualisierte Definition für die `Team`-Klasse sowie einige weitere Entity Framework-Hilfsklassen. Weitere Informationen zum in diesem Tutorial verwendeten Code First-Ansatz für Entity Framework finden Sie unter [Code First für eine neue Datenbank](https://msdn.microsoft.com/data/jj193542).

    ```c#
    public class Team
    {
        public int ID { get; set; }
        public string Name { get; set; }
        public int Wins { get; set; }
        public int Losses { get; set; }
        public int Ties { get; set; }
    
        static public void PlayGames(IEnumerable<Team> teams)
        {
            // Simple random generation of statistics.
            Random r = new Random();
    
            foreach (var t in teams)
            {
                t.Wins = r.Next(33);
                t.Losses = r.Next(33);
                t.Ties = r.Next(0, 5);
            }
        }
    }
    
    public class TeamContext : DbContext
    {
        public TeamContext()
            : base("TeamContext")
        {
        }
    
        public DbSet<Team> Teams { get; set; }
    }
    
    public class TeamInitializer : CreateDatabaseIfNotExists<TeamContext>
    {
        protected override void Seed(TeamContext context)
        {
            var teams = new List<Team>
            {
                new Team{Name="Adventure Works Cycles"},
                new Team{Name="Alpine Ski House"},
                new Team{Name="Blue Yonder Airlines"},
                new Team{Name="Coho Vineyard"},
                new Team{Name="Contoso, Ltd."},
                new Team{Name="Fabrikam, Inc."},
                new Team{Name="Lucerne Publishing"},
                new Team{Name="Northwind Traders"},
                new Team{Name="Consolidated Messenger"},
                new Team{Name="Fourth Coffee"},
                new Team{Name="Graphic Design Institute"},
                new Team{Name="Nod Publishers"}
            };
    
            Team.PlayGames(teams);
    
            teams.ForEach(t => context.Teams.Add(t));
            context.SaveChanges();
        }
    }
    
    public class TeamConfiguration : DbConfiguration
    {
        public TeamConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
        }
    }
    ```


1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei **web.config**, um sie zu öffnen.
   
    ![Web.config][cache-web-config]
2. Fügen Sie den folgenden `connectionStrings`-Abschnitt hinzu. Der Name der Verbindungszeichenfolge muss dem Namen der Entity Framework-Datenbankkontextklasse ( `TeamContext`) entsprechen.

    ```xml
    <connectionStrings>
        <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"     providerName="System.Data.SqlClient" />
    </connectionStrings>
    ```

    Sie können den neuen `connectionStrings`-Abschnitt so hinzufügen, dass er auf `configSections` folgt. Dies ist im folgenden Beispiel dargestellt.

    ```xml
    <configuration>
      <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
      </configSections>
      <connectionStrings>
        <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"     providerName="System.Data.SqlClient" />
      </connectionStrings>
      ...
      ```

### <a name="add-the-controller"></a>Hinzufügen des Controllers
1. Drücken Sie F6 **** , um das Projekt zu erstellen. 
2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Controller**, und wählen Sie anschließend **Hinzufügen** > **Controller** aus.
   
    ![Controller hinzufügen][cache-add-controller]
3. Wählen Sie **MVC 5-Controller mit Ansichten per Entity Framework** aus, und klicken Sie auf **Hinzufügen**. Sollte nach dem Klicken auf **Hinzufügen**eine Fehlermeldung angezeigt werden, vergewissern Sie sich, dass das Projekt erstellt wurde.
   
    ![Controllerklasse hinzufügen][cache-add-controller-class]
4. Wählen Sie in der Dropdownliste **Modellklasse** die Option **Team (ContosoTeamStats.Models)** aus. Wählen Sie in der Dropdownliste **Datenkontextklasse** die Option **TeamContext (ContosoTeamStats.Models)** aus. Geben Sie im Textfeld **Controllername** die Zeichenfolge `TeamsController` ein (sofern sie nicht automatisch eingefügt wird). Klicken Sie auf **Hinzufügen** , um die Controllerklasse zu erstellen und die Standardansichten hinzuzufügen.
   
    ![Controller konfigurieren][cache-configure-controller]
5. Erweitern Sie im **Projektmappen-Explorer** den Knoten **Global.asax**, und doppelklicken Sie anschließend auf **Global.asax.cs**, um die Datei zu öffnen.
   
    ![Global.asax.cs][cache-global-asax]
6. Fügen Sie den `using`-Anweisungen am Anfang der Datei die beiden folgenden `using`-Anweisungen hinzu:

    ```c#
    using System.Data.Entity;
    using ContosoTeamStats.Models;
    ```


1. Fügen Sie am Ende der `Application_Start` -Methode folgenden Code hinzu:

    ```c#
    Database.SetInitializer<TeamContext>(new TeamInitializer());
    ```


1. Erweitern Sie im **Projektmappen-Explorer** den Knoten `App_Start`, und doppelklicken Sie auf `RouteConfig.cs`.
   
    ![RouteConfig.cs][cache-RouteConfig-cs]
2. Ersetzen Sie `controller = "Home"` im folgenden Code der `RegisterRoutes`-Methode durch `controller = "Teams"`, wie im folgenden Beispiel gezeigt:

    ```c#
    routes.MapRoute(
        name: "Default",
        url: "{controller}/{action}/{id}",
        defaults: new { controller = "Teams", action = "Index", id = UrlParameter.Optional }
    );
```


### <a name="configure-the-views"></a>Konfigurieren der Ansichten
1. Erweitern Sie im **Projektmappen-Explorer** den Ordner **Ansichten**, erweitern Sie den Ordner **Freigegeben**, und doppelklicken Sie auf **_Layout.cshtml**. 
   
    ![_Layout.cshtml][cache-layout-cshtml]
2. Ändern Sie den Inhalt des `title`-Elements, und ersetzen Sie `My ASP.NET Application` durch `Contoso Team Stats`, wie im folgenden Beispiel gezeigt:

    ```html
    <title>@ViewBag.Title - Contoso Team Stats</title>
    ```


1. Aktualisieren Sie im `body`-Abschnitt die erste `Html.ActionLink`-Anweisung, und ersetzen Sie `Application name` durch `Contoso Team Stats` und `Home` durch `Teams`.
   
   * Vorher: `@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`
   * Nachher: `@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`
     
     ![Änderungen am Code][cache-layout-cshtml-code]
2. Drücken Sie **STRG+F5** , um die Anwendung zu erstellen und auszuführen. Diese Version der Anwendung liest die Ergebnisse direkt aus der Datenbank. Die Aktionen **Neu erstellen**, **Bearbeiten**, **Details** und **Löschen** wurden der Anwendung aufgrund des Gerüsts **MVC 5-Controller mit Ansichten per Entity Framework** automatisch hinzugefügt. Im nächsten Abschnitt des Tutorials wird Redis Cache hinzugefügt, um den Datenzugriff zu optimieren und zusätzliche Features für die Anwendung bereitzustellen.

![Starter-Anwendung][cache-starter-application]

## <a name="configure-the-application-to-use-redis-cache"></a>Konfigurieren der Anwendung für die Verwendung von Redis Cache
In diesem Abschnitt des Tutorials wird die Beispielanwendung zum Speichern und Abrufen von Contoso-Teamstatistikdaten aus einer Azure Redis Cache-Instanz unter Verwendung des Cacheclients [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) konfiguriert.

* [Konfigurieren der Anwendung für die Verwendung von „StackExchange.Redis“](#configure-the-application-to-use-stackexchangeredis)
* [Aktualisieren der TeamsController-Klasse zum Zurückgeben von Ergebnissen aus dem Cache oder der Datenbank](#update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database)
* [Aktualisieren der Create-, Edit- und Delete-Methoden für den Cache](#update-the-create-edit-and-delete-methods-to-work-with-the-cache)
* [Aktualisieren der Teams-Indexansicht für den Cache](#update-the-teams-index-view-to-work-with-the-cache)

### <a name="configure-the-application-to-use-stackexchangeredis"></a>Konfigurieren der Anwendung für die Verwendung von „StackExchange.Redis“
1. Gehen Sie wie folgt vor, um eine Clientanwendung in Visual Studio mit dem StackExchange.Redis-NuGet-Paket zu konfigurieren: Klicken Sie im Menü **Extras** auf **NuGet-Paket-Manager** > **Paket-Manager-Konsole**.
2. Führen Sie im Fenster `Package Manager Console` den folgenden Befehl aus:
    
    ```
    Install-Package StackExchange.Redis
    ```
   
    Das NuGet-Paket wird heruntergeladen und fügt die benötigten Assemblyverweise zu Ihrer Clientanwendung hinzu, um mithilfe des StackExchange.Redis-Cacheclients auf den Azure Redis Cache zuzugreifen. Installieren Sie das Paket `StackExchange.Redis.StrongName`, wenn Sie es vorziehen, eine Version der `StackExchange.Redis`-Clientbibliothek mit sicheren Namen zu verwenden.
3. Erweitern Sie im **Projektmappen-Explorer** den Ordner **Controller**, und doppelklicken Sie auf **TeamsController.cs**, um die Datei zu öffnen.
   
    ![Teamcontroller][cache-teamscontroller]
4. Fügen Sie **TeamsController.cs** die beiden folgenden `using`-Anweisungen hinzu:

    ```c#   
    using System.Configuration;
    using StackExchange.Redis;
    ```

5. Fügen Sie der `TeamsController` -Klasse die beiden folgenden Eigenschaften hinzu:

    ```c#   
    // Redis Connection string info
    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
        return ConnectionMultiplexer.Connect(cacheConnection);
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }
    ```

6. Erstellen Sie auf Ihrem Computer eine Datei namens `WebAppPlusCacheAppSecrets.config` , und platzieren Sie sie an einem Ort, der nicht mit dem Quellcode Ihrer Beispielanwendung eingecheckt wird, falls Sie beschließen, ihn irgendwo einzuchecken. In diesem Beispiel befindet sich die Datei `AppSettingsSecrets.config` unter `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.
   
    Bearbeiten Sie die Datei `WebAppPlusCacheAppSecrets.config`, und fügen Sie ihr folgende Inhalte hinzu. Wenn Sie die Anwendung lokal ausführen, werden diese Informationen verwendet, um eine Verbindung mit Ihrer Azure Redis Cache-Instanz herzustellen. Im weiteren Verlauf dieses Tutorials stellen Sie eine Azure Redis Cache-Instanz bereit und aktualisieren den Cachenamen und das Kennwort. Wenn Sie die Beispielanwendung nicht lokal ausführen, können Sie die Erstellung dieser Datei und die nachfolgenden Schritte, die sich auf die Datei beziehen, überspringen. Bei einer Bereitstellung für Azure ruft die Anwendung die Cacheverbindungsinformationen nicht aus dieser Datei, sondern aus der App-Einstellung für die Web-App ab. Da `WebAppPlusCacheAppSecrets.config` nicht mit Ihrer Anwendung für Azure bereitgestellt wird, benötigen Sie sie nur, wenn Sie die Anwendung lokal ausführen möchten.

    ```xml
    <appSettings>
      <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
    </appSettings>
    ```


1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei **web.config**, um sie zu öffnen.
   
    ![Web.config][cache-web-config]
2. Fügen Sie dem `appSettings`-Element das folgende `file`-Attribut hinzu. Falls Sie einen anderen Dateinamen oder -speicherort verwendet haben, müssen die Werte aus dem Beispiel durch diese Werte ersetzt werden.
   
   * Vorher: `<appSettings>`
   * Nachher: ` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`
     
   Die ASP.NET-Laufzeit führt die Inhalte der externen Datei mit dem Markup im `<appSettings>`-Element zusammen. Falls die angegebene Datei nicht gefunden wird, wird das Dateiattribut ignoriert. Ihre vertraulichen Daten (die Verbindungszeichenfolge für Ihren Cache) sind nicht Bestandteil des Quellcodes für die Anwendung. Wenn Sie Ihre Web-App für Azure bereitstellen, wird die Datei `WebAppPlusCacheAppSecrests.config` nicht bereitgestellt. (Dieses Verhalten ist so beabsichtigt.) Es gibt mehrere Möglichkeiten, die vertraulichen Daten in Azure anzugeben. In diesem Tutorial werden sie automatisch für Sie konfiguriert, wenn Sie in einem späteren Schritt des Tutorials die [Azure-Ressourcen bereitstellen](#provision-the-azure-resources). Weitere Informationen zur Verwendung vertraulicher Daten in Azure finden Sie unter [Best Practices für das Bereitstellen von Kennwörtern und anderen vertraulichen Daten in ASP.NET und Azure App Service](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).

### <a name="update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database"></a>Aktualisieren der TeamsController-Klasse zum Zurückgeben von Ergebnissen aus dem Cache oder der Datenbank
In diesem Beispiel können Teamstatistikdaten aus der Datenbank oder aus dem Cache abgerufen werden. Teamstatistikdaten werden im Cache als serialisierte Daten vom Typ `List<Team>`sowie als sortierter Satz mit Redis-Datentypen gespeichert. Beim Abrufen von Elementen aus einem sortierten Satz können Sie einige oder alle Elemente abrufen oder bestimmte Elemente abfragen. Im vorliegenden Beispiel werden die fünf besten Teams aus dem sortierten Satz abgefragt und nach der Anzahl gewonnener Spiele sortiert.

> [!NOTE]
> Die Teamstatistik muss zur Verwendung von Azure Redis Cache nicht in mehreren Formaten im Cache gespeichert werden. Dieses Tutorial verwendet mehrere Formate, um verschiedene Möglichkeiten und Datentypen für die Zwischenspeicherung von Daten zu veranschaulichen.
> 
> 

1. Fügen Sie den `using`-Anweisungen am Anfang der Datei `TeamsController.cs` folgende weitere `using`-Anweisungen hinzu:

    ```c#   
    using System.Diagnostics;
    using Newtonsoft.Json;
    ```

2. Ersetzen Sie die aktuelle Implementierung der `public ActionResult Index()`-Methode durch die folgende Implementierung:

    ```c#
    // GET: Teams
    public ActionResult Index(string actionType, string resultType)
    {
        List<Team> teams = null;

        switch(actionType)
        {
            case "playGames": // Play a new season of games.
                PlayGames();
                break;

            case "clearCache": // Clear the results from the cache.
                ClearCachedTeams();
                break;

            case "rebuildDB": // Rebuild the database with sample data.
                RebuildDB();
                break;
        }

        // Measure the time it takes to retrieve the results.
        Stopwatch sw = Stopwatch.StartNew();

        switch(resultType)
        {
            case "teamsSortedSet": // Retrieve teams from sorted set.
                teams = GetFromSortedSet();
                break;

            case "teamsSortedSetTop5": // Retrieve the top 5 teams from the sorted set.
                teams = GetFromSortedSetTop5();
                break;

            case "teamsList": // Retrieve teams from the cached List<Team>.
                teams = GetFromList();
                break;

            case "fromDB": // Retrieve results from the database.
            default:
                teams = GetFromDB();
                break;
        }

        sw.Stop();
        double ms = sw.ElapsedTicks / (Stopwatch.Frequency / (1000.0));

        // Add the elapsed time of the operation to the ViewBag.msg.
        ViewBag.msg += " MS: " + ms.ToString();

        return View(teams);
    }
    ```


1. Fügen Sie der `TeamsController`-Klasse die drei folgenden Methoden hinzu, um die Aktionstypen `playGames`, `clearCache` und `rebuildDB` aus der im vorherigen Codeausschnitt hinzugefügten switch-Anweisung zu implementieren.
   
    Die `PlayGames` -Methode aktualisiert die Teamstatistik durch eine simulierte Spielesaison, speichert die Ergebnisse in der Datenbank und löscht die inzwischen veralteten Daten aus dem Cache.

    ```c#
    void PlayGames()
    {
        ViewBag.msg += "Updating team statistics. ";
        // Play a "season" of games.
        var teams = from t in db.Teams
                    select t;

        Team.PlayGames(teams);

        db.SaveChanges();

        // Clear any cached results
        ClearCachedTeams();
    }
    ```

    Die `RebuildDB` -Methode initialisiert die Datenbank mit dem Standardteamsatz neu, generiert die Statistik für die Teams und löscht die nun veralteten Daten aus dem Cache.

    ```c#
    void RebuildDB()
    {
        ViewBag.msg += "Rebuilding DB. ";
        // Delete and re-initialize the database with sample data.
        db.Database.Delete();
        db.Database.Initialize(true);

        // Clear any cached results
        ClearCachedTeams();
    }
    ```

    Die `ClearCachedTeams` -Methode entfernt alle zwischengespeicherten Teamstatistikdaten aus dem Cache.

    ```c#
    void ClearCachedTeams()
    {
        IDatabase cache = Connection.GetDatabase();
        cache.KeyDelete("teamsList");
        cache.KeyDelete("teamsSortedSet");
        ViewBag.msg += "Team data removed from cache. ";
    } 
    ```


1. Fügen Sie der `TeamsController` -Klasse die vier folgenden Methoden hinzu, um die verschiedenen Methoden zum Abrufen der Teamstatistik aus dem Cache und der Datenbank zu implementieren. Jede dieser Methoden gibt ein Element vom Typ `List<Team>` zurück, das dann in der Ansicht angezeigt wird.
   
    Die `GetFromDB` -Methode liest die Teamstatistik aus der Datenbank.
   
    ```c#
    List<Team> GetFromDB()
    {
        ViewBag.msg += "Results read from DB. ";
        var results = from t in db.Teams
            orderby t.Wins descending
            select t; 

        return results.ToList<Team>();
    }
    ```

    Die `GetFromList`-Methode liest die Teamstatistik aus dem Cache als serialisierte Daten vom Typ `List<Team>`. Bei einem Cachefehler wird die Teamstatistik aus der Datenbank gelesen und für den nächsten Zugriff im Cache gespeichert. In diesem Beispiel verwenden wir die JSON.NET-Serialisierung, um die .NET-Objekte für den Cache zu serialisieren. Weitere Informationen finden Sie unter [Arbeiten mit .NET-Objekten im Cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

    ```c#
    List<Team> GetFromList()
    {
        List<Team> teams = null;

        IDatabase cache = Connection.GetDatabase();
        string serializedTeams = cache.StringGet("teamsList");
        if (!String.IsNullOrEmpty(serializedTeams))
        {
            teams = JsonConvert.DeserializeObject<List<Team>>(serializedTeams);

            ViewBag.msg += "List read from cache. ";
        }
        else
        {
            ViewBag.msg += "Teams list cache miss. ";
            // Get from database and store in cache
            teams = GetFromDB();

            ViewBag.msg += "Storing results to cache. ";
            cache.StringSet("teamsList", JsonConvert.SerializeObject(teams));
        }
        return teams;
    }
    ```

    Die `GetFromSortedSet` -Methode liest die Teamstatistik aus einem zwischengespeicherten sortierten Satz. Bei einem Cachefehler wird die Teamstatistik aus der Datenbank gelesen und im Cache als sortierter Satz gespeichert.

    ```c#
    List<Team> GetFromSortedSet()
    {
        List<Team> teams = null;
        IDatabase cache = Connection.GetDatabase();
        // If the key teamsSortedSet is not present, this method returns a 0 length collection.
        var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", order: Order.Descending);
        if (teamsSortedSet.Count() > 0)
        {
            ViewBag.msg += "Reading sorted set from cache. ";
            teams = new List<Team>();
            foreach (var t in teamsSortedSet)
            {
                Team tt = JsonConvert.DeserializeObject<Team>(t.Element);
                teams.Add(tt);
            }
        }
        else
        {
            ViewBag.msg += "Teams sorted set cache miss. ";

            // Read from DB
            teams = GetFromDB();

            ViewBag.msg += "Storing results to cache. ";
            foreach (var t in teams)
            {
                Console.WriteLine("Adding to sorted set: {0} - {1}", t.Name, t.Wins);
                cache.SortedSetAdd("teamsSortedSet", JsonConvert.SerializeObject(t), t.Wins);
            }
        }
        return teams;
    }
    ```

    Die `GetFromSortedSetTop5` -Methode liest die fünf besten Teams aus dem zwischengespeicherten sortierten Satz. Dabei wird zunächst geprüft, ob der `teamsSortedSet` -Schlüssel im Cache vorhanden ist. Ist der Schlüssel nicht vorhanden, wird die `GetFromSortedSet` -Methode aufgerufen, um die Teamstatistik zu lesen und im Cache zu speichern. Als Nächstes werden aus dem zwischengespeicherten sortierten Satz die fünf besten Teams abgefragt und zurückgegeben.

    ```c#
    List<Team> GetFromSortedSetTop5()
    {
        List<Team> teams = null;
        IDatabase cache = Connection.GetDatabase();

        // If the key teamsSortedSet is not present, this method returns a 0 length collection.
        var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
        if(teamsSortedSet.Count() == 0)
        {
            // Load the entire sorted set into the cache.
            GetFromSortedSet();

            // Retrieve the top 5 teams.
            teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
        }

        ViewBag.msg += "Retrieving top 5 teams from cache. ";
        // Get the top 5 teams from the sorted set
        teams = new List<Team>();
        foreach (var team in teamsSortedSet)
        {
            teams.Add(JsonConvert.DeserializeObject<Team>(team.Element));
        }
        return teams;
    }
    ```

### <a name="update-the-create-edit-and-delete-methods-to-work-with-the-cache"></a>Aktualisieren der Create-, Edit- und Delete-Methoden für den Cache
Der im Rahmen dieses Beispiels generierte Gerüstcode enthält Methoden zum Hinzufügen, Bearbeiten und Löschen. Sobald ein Team hinzugefügt, bearbeitet oder entfernt wurde, sind die Daten im Cache veraltet. In diesem Abschnitt werden diese drei Methoden geändert, um die zwischengespeicherten Teams zu löschen und dadurch zu verhindern, dass der Cache Daten enthält, die nicht mit der Datenbank synchronisiert sind.

1. Navigieren Sie in der `TeamsController`-Klasse zur `Create(Team team)`-Methode. Fügen Sie einen Aufruf der `ClearCachedTeams` -Methode hinzu, wie im folgenden Beispiel zu sehen:

    ```c#
    // POST: Teams/Create
    // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
    // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
    [HttpPost]
    [ValidateAntiForgeryToken]
    public ActionResult Create([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
    {
        if (ModelState.IsValid)
        {
            db.Teams.Add(team);
            db.SaveChanges();
            // When a team is added, the cache is out of date.
            // Clear the cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }

        return View(team);
    }
    ```


1. Navigieren Sie in der `TeamsController`-Klasse zur `Edit(Team team)`-Methode. Fügen Sie einen Aufruf der `ClearCachedTeams` -Methode hinzu, wie im folgenden Beispiel zu sehen:

    ```c#
    // POST: Teams/Edit/5
    // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
    // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
    [HttpPost]
    [ValidateAntiForgeryToken]
    public ActionResult Edit([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
    {
        if (ModelState.IsValid)
        {
            db.Entry(team).State = EntityState.Modified;
            db.SaveChanges();
            // When a team is edited, the cache is out of date.
            // Clear the cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }
        return View(team);
    }
    ```


1. Navigieren Sie in der `TeamsController`-Klasse zur `DeleteConfirmed(int id)`-Methode. Fügen Sie einen Aufruf der `ClearCachedTeams` -Methode hinzu, wie im folgenden Beispiel zu sehen:

    ```c#
    // POST: Teams/Delete/5
    [HttpPost, ActionName("Delete")]
    [ValidateAntiForgeryToken]
    public ActionResult DeleteConfirmed(int id)
    {
        Team team = db.Teams.Find(id);
        db.Teams.Remove(team);
        db.SaveChanges();
        // When a team is deleted, the cache is out of date.
        // Clear the cached teams.
        ClearCachedTeams();
        return RedirectToAction("Index");
    }
    ```


### <a name="update-the-teams-index-view-to-work-with-the-cache"></a>Aktualisieren der Teams-Indexansicht für den Cache
1. Erweitern Sie im **Projektmappen-Explorer** den Ordner **Ansichten**, erweitern Sie den Ordner **Teams**, und doppelklicken Sie auf **Index.cshtml**.
   
    ![Index.cshtml][cache-views-teams-index-cshtml]
2. Suchen Sie im oberen Bereich der Datei nach dem folgenden Absatzelement:
   
    ![Aktionentabelle][cache-teams-index-table]
   
    Dies ist der Link zum Erstellen eines neuen Teams. Ersetzen Sie das Absatzelement durch die folgende Tabelle. Die Tabelle enthält Aktionslinks zum Erstellen eines neuen Teams, zum Spielen einer neuen Saison, zum Löschen des Cache, zum Abrufen der Teams aus dem Cache in unterschiedlichen Formaten, zum Abrufen der Teams aus der Datenbank sowie zum Neuerstellen der Datenbank mit frischen Beispieldaten.

    ```html
    <table class="table">
        <tr>
            <td>
                @Html.ActionLink("Create New", "Create")
            </td>
            <td>
                @Html.ActionLink("Play Season", "Index", new { actionType = "playGames" })
            </td>
            <td>
                @Html.ActionLink("Clear Cache", "Index", new { actionType = "clearCache" })
            </td>
            <td>
                @Html.ActionLink("List from Cache", "Index", new { resultType = "teamsList" })
            </td>
            <td>
                @Html.ActionLink("Sorted Set from Cache", "Index", new { resultType = "teamsSortedSet" })
            </td>
            <td>
                @Html.ActionLink("Top 5 Teams from Cache", "Index", new { resultType = "teamsSortedSetTop5" })
            </td>
            <td>
                @Html.ActionLink("Load from DB", "Index", new { resultType = "fromDB" })
            </td>
            <td>
                @Html.ActionLink("Rebuild DB", "Index", new { actionType = "rebuildDB" })
            </td>
        </tr>    
    </table>
    ```


1. Führen Sie einen Bildlauf zum unteren Rand der Datei **Index.cshtml** durch, und fügen Sie das folgende `tr`-Element als letzte Zeile der letzten Tabelle in der Datei hinzu:
   
    ```html
    <tr><td colspan="5">@ViewBag.Msg</td></tr>
    ```
   
    In dieser Zeile wird der Wert `ViewBag.Msg` angezeigt, der einen Statusbericht zum aktuellen Vorgang enthält. `ViewBag.Msg` wird festgelegt, wenn Sie auf einen der Aktionslinks aus dem vorherigen Schritt klicken.   
   
    ![Statusmeldung][cache-status-message]
2. Drücken Sie F6 **** , um das Projekt zu erstellen.

## <a name="provision-the-azure-resources"></a>Bereitstellen der Azure-Ressourcen
Bevor Sie Ihre Anwendung in Azure hosten können, müssen Sie die erforderlichen Azure-Dienste bereitstellen. Die Beispielanwendung in diesem Tutorial verwendet die folgenden Azure-Dienste:

* Azure Redis Cache
* App Service-Web-App
* SQL-Datenbank

Klicken Sie auf die folgende Schaltfläche **** , um diese Dienste für eine neue oder vorhandene Ressourcengruppe Ihrer Wahl bereitzustellen:

[![Bereitstellung in Azure][deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)

Die Schaltfläche zum ****Bereitstellen für Azure verwendet die [Azure-Schnellstartvorlage](https://github.com/Azure/azure-quickstart-templates) namens [Create a Web App plus Redis Cache plus SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database), um diese Dienste bereitzustellen und die Verbindungszeichenfolge für die SQL-Datenbank sowie die Anwendungseinstellung für die Azure Redis Cache-Verbindungszeichenfolge festzulegen.

> [!NOTE]
> Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Azure-Konto erstellen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero) .
> 
> 

Nach dem Klicken auf die Schaltfläche zum Bereitstellen für Azure **** gelangen Sie zum Azure-Portal, und die Erstellung der in der Vorlage beschriebenen Ressourcen wird initiiert.

![Bereitstellen in Azure][cache-deploy-to-azure-step-1]

1. Wählen Sie im Abschnitt **Grundlagen** das gewünschte Azure-Abonnement und dann eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue, und geben Sie den Speicherort der Ressourcengruppe an.
2. Geben Sie im Abschnitt **Einstellungen** eine **Administratoranmeldung** (nicht **admin**), ein **Kennwort für die Administratoranmeldung** und einen **Datenbanknamen** ein. Die anderen Parameter sind für einen kostenlosen App Service-Hostingtarif (Free-Tarif) sowie mit kostengünstigen Optionen für SQL-Datenbank und Azure Redis Cache konfiguriert, die im Free-Tarif nicht enthalten sind.

    ![Bereitstellen in Azure][cache-deploy-to-azure-step-2]

3. Nach dem Konfigurieren der gewünschten Einstellungen führen Sie einen Bildlauf zum Ende der Seite durch, lesen die Geschäftsbedingungen und aktivieren das Kontrollkästchen **Ich stimme den oben genannten Geschäftsbedingungen zu**.
4. Klicken Sie zum Starten der Ressourcenbereitstellung auf **Kaufen**.

Klicken Sie auf das Benachrichtigungssymbol und anschließend auf **Bereitstellung gestartet...**, um den Status der Bereitstellung anzuzeigen.

![Bereitstellung gestartet...][cache-deployment-started]

Der Status Ihrer Bereitstellung wird auf dem Blatt **Microsoft.Template** angezeigt.

![Bereitstellen in Azure][cache-deploy-to-azure-step-3]

Nach Abschluss der Bereitstellung können Sie Ihre Anwendung über Visual Studio für Azure veröffentlichen.

> [!NOTE]
> Während der Bereitstellung auftretende Fehler werden auf dem Blatt **Microsoft.Template** angezeigt. Die Fehler sind häufig auf zu viele SQL Server oder auf zu viele kostenlose App Service-Hostingtarife pro Abonnement zurückzuführen. Beheben Sie alle Fehler, und starten Sie den Prozess erneut, indem Sie auf dem Blatt **Microsoft.Template** auf **Erneut bereitstellen** oder in diesem Tutorial auf die Schaltfläche zum Bereitstellen für Azure**** klicken.
> 
> 

## <a name="publish-the-application-to-azure"></a>Veröffentlichen der Anwendung für Azure
In diesem Schritt des Tutorials wird Anwendung für Azure veröffentlicht und in der Cloud ausgeführt.

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt **ContosoTeamStats**, und wählen Sie **Veröffentlichen**.
   
    ![Veröffentlichen][cache-publish-app]
2. Klicken Sie auf **Microsoft Azure App Service**, wählen Sie **Vorhandene auswählen**, und klicken Sie auf **Veröffentlichen**.
   
    ![Veröffentlichen][cache-publish-to-app-service]
3. Wählen Sie das Abonnement aus, das beim Erstellen der Azure-Ressourcen verwendet wurde, erweitern Sie die Ressourcengruppe mit den Ressourcen, und wählen Sie die gewünschte Web-App aus. Bei Verwendung der Schaltfläche zum Bereitstellen für Azure**** beginnt der Name Ihrer Web-App mit **webSite**, gefolgt von einigen zusätzlichen Zeichen.
   
    ![Web-App auswählen][cache-select-web-app]
4. Klicken Sie auf **OK**, um mit dem Veröffentlichungsprozess zu beginnen. Der Veröffentlichungsprozess wird kurz darauf abgeschlossen, und die Beispielanwendung wird in einem Browser gestartet. Falls bei der Überprüfung oder Veröffentlichung ein DNS-Fehler auftritt und der Bereitstellungsprozess der Azure-Ressourcen für die Anwendung erst vor Kurzem abgeschlossen wurde, warten Sie einen Moment, und versuchen Sie es dann noch einmal.
   
    ![Cache hinzugefügt][cache-added-to-application]

In der folgenden Tabelle werden die einzelnen Aktionslinks aus der Beispielanwendung beschrieben:

| Aktion | Beschreibung |
| --- | --- |
| Create New |Erstellt ein neues Team. |
| Play Season |Spielt eine Spielesaison durch, aktualisiert die Teamstatistik und entfernt veraltete Teamdaten aus dem Cache. |
| Clear Cache |Löscht die Teamstatistik aus dem Cache. |
| List from Cache |Ruft die Teamstatistik aus dem Cache ab. Bei einem Cachefehler wird die Teamstatistik aus der Datenbank geladen und für den nächsten Zugriff im Cache gespeichert. |
| Sorted Set from Cache |Ruft die Teamstatistik unter Verwendung eines sortierten Satzes aus dem Cache ab. Bei einem Cachefehler wird die Teamstatistik aus der Datenbank geladen und unter Verwendung eines sortierten Satzes im Cache gespeichert. |
| Top 5 Teams from Cache |Ruft die fünf besten Teams unter Verwendung eines sortierten Satzes aus dem Cache ab. Bei einem Cachefehler wird die Teamstatistik aus der Datenbank geladen und unter Verwendung eines sortierten Satzes im Cache gespeichert. |
| Load from DB |Ruft die Teamstatistik aus der Datenbank ab. |
| Rebuild DB |Erstellt die Datenbank neu und lädt sie mit Beispielteamdaten. |
| Edit/Details/Delete |Dient zum Bearbeiten eines Teams, zum Anzeigen von Teamdetails bzw. zum Löschen eines Teams. |

Klicken Sie auf einige der Aktionen, und experimentieren Sie mit dem Abrufen der Daten aus unterschiedlichen Quellen. Beachten Sie jeweils die unterschiedliche Vorgangsdauer bei den verschiedenen Methoden zum Abrufen von Daten aus der Datenbank und dem Cache.

## <a name="delete-the-resources-when-you-are-finished-with-the-application"></a>Löschen der Ressourcen, wenn Sie die Anwendung nicht mehr benötigen
Wenn Sie die Beispielanwendung des Tutorials nicht mehr benötigen, können Sie die Azure-Ressourcen löschen, um Kosten und Ressourcen zu sparen. Wenn Sie im Abschnitt [Bereitstellen der Azure-Ressourcen](#provision-the-azure-resources) die Schaltfläche zum Bereitstellen für Azure**** verwendet haben und sich Ihre Ressourcen alle in der gleichen Ressourcengruppe befinden, können Sie sie durch Löschen der Ressourcengruppe alle auf einmal löschen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und klicken Sie auf **Ressourcengruppen**.
2. Geben Sie im Textfeld **Filterelemente...** den Namen Ihrer Ressourcengruppe ein.
3. Klicken Sie rechts neben Ihrer Ressourcengruppe auf **...** .
4. Klicken Sie auf **Löschen**.
   
    ![Löschen][cache-delete-resource-group]
5. Geben Sie den Namen Ihrer Ressourcengruppe ein, und klicken Sie auf **Löschen**.
   
    ![Bestätigen des Löschens][cache-delete-confirm]

Daraufhin werden die Ressourcengruppe und alle darin enthaltenen Ressourcen gelöscht.

> [!IMPORTANT]
> Hinweis: Das Löschen einer Ressourcengruppe kann nicht rückgängig gemacht werden. Die Ressourcengruppe und alle darin enthaltenen Ressourcen werden also unwiederbringlich gelöscht. Achten Sie daher darauf, dass Sie nicht versehentlich die falsche Ressourcengruppe oder die falschen Ressourcen löschen. Wenn Sie die Ressourcen zum Hosten dieses Beispiels in einer bereits zuvor vorhandenen Ressourcengruppe erstellt haben, können Sie sie einzeln über die entsprechenden Blätter löschen.
> 
> 

## <a name="run-the-sample-application-on-your-local-machine"></a>Ausführen der Beispielanwendung auf Ihrem lokalen Computer
Wenn Sie die Anwendung lokal auf Ihrem Computer ausführen möchten, benötigen Sie eine Azure Redis Cache-Instanz, in der Sie Ihre Daten zwischenspeichern können. 

* Wenn Sie Ihre Anwendung wie im vorherigen Abschnitt für Azure veröffentlicht haben, können Sie die Azure Redis Cache-Instanz verwenden, die in diesem Schritt bereitgestellt wurde.
* Falls Sie über eine andere vorhandene Azure Redis Cache-Instanz verfügen, können Sie auch diese Instanz verwenden, um das Beispiel lokal auszuführen.
* Eine Anleitung zum Erstellen einer Azure Redis Cache-Instanz finden Sie unter [Erstellen eines Caches](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

Nachdem Sie den zu verwendenden Cache ausgewählt oder erstellt haben, navigieren Sie im Azure-Portal zum Cache, und rufen Sie [Hostname](cache-configure.md#properties) und [Zugriffsschlüssel](cache-configure.md#access-keys) für Ihren Cache ab. Eine entsprechende Anleitung finden Sie unter [Konfigurieren von Azure Redis Cache](cache-configure.md#configure-redis-cache-settings).

1. Öffnen Sie die Datei `WebAppPlusCacheAppSecrets.config` , die Sie in diesem Tutorial beim [Konfigurieren der Anwendung für die Verwendung von Redis Cache](#configure-the-application-to-use-redis-cache) erstellt haben, mit einem beliebigen Editor.
2. Bearbeiten Sie das `value`-Attribut, ersetzen Sie `MyCache.redis.cache.windows.net` durch den [Hostnamen](cache-configure.md#properties) Ihres Cache, und geben Sie den [primären oder sekundären Schlüssel](cache-configure.md#access-keys) des Cache als Kennwort an.

    ```xml
    <appSettings>
      <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
    </appSettings>
    ```


1. Drücken Sie **STRG+F5** , um die Anwendung auszuführen.

> [!NOTE]
> Da die Anwendung einschließlich der Datenbank lokal ausgeführt und der Redis Cache in Azure gehostet wird, entsteht unter Umständen der Eindruck, die Datenbank sei dem Cache leistungsmäßig überlegen. Zur Optimierung der Leistung müssen sich die Clientanwendung und die Azure Redis Cache-Instanz am gleichen Ort befinden. 
> 
> 

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich auf der [ASP.NET](http://asp.net/)-Website ausführlicher über die [ersten Schritte mit ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started).
* Weitere Beispiele zum Erstellen einer ASP.NET-Web-App in App Service finden Sie in der [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect-[Demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/) unter [Create and deploy an ASP.NET web app in Azure App Service](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) (Erstellen und Bereitstellen einer ASP.NET-Web-App in Azure App Service).
  * Weitere Schnellstarts aus der HealthClinic.biz-Demo finden Sie unter [Azure Developer Tools Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts)(Schnellstarts zu Azure-Entwicklungstools).
* Beschäftigen Sie sich unter [Code First für eine neue Datenbank](https://msdn.microsoft.com/data/jj193542) ausführlicher mit dem in diesem Tutorial verwendeten Code First-Ansatz für Entity Framework.
* Machen Sie sich ausführlicher mit [Web-Apps in Azure App Service](../app-service-web/app-service-web-overview.md)vertraut.
* Informieren Sie sich, wie Sie Ihren Cache im Azure-Portal [überwachen](cache-how-to-monitor.md) .
* Machen Sie sich mit den Premium-Features von Azure Redis Cache vertraut:
  
  * [Konfigurieren von Persistenz für Azure Redis Cache vom Typ "Premium"](cache-how-to-premium-persistence.md)
  * [Konfigurieren von Clustern für Azure Redis Cache vom Typ "Premium"](cache-how-to-premium-clustering.md)
  * [Konfigurieren der Unterstützung virtueller Netzwerke für Azure Redis Cache vom Typ "Premium"](cache-how-to-premium-vnet.md)
  * Ausführliche Informationen zu Größe, Durchsatz und Bandbreite von Premium-Caches finden Sie in den [häufig gestellten Fragen zu Azure Redis Cache](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) .

<!-- IMAGES -->
[cache-starter-application]: ./media/cache-web-app-howto/cache-starter-application.png
[cache-added-to-application]: ./media/cache-web-app-howto/cache-added-to-application.png
[cache-create-project]: ./media/cache-web-app-howto/cache-create-project.png
[cache-select-template]: ./media/cache-web-app-howto/cache-select-template.png
[cache-model-add-class]: ./media/cache-web-app-howto/cache-model-add-class.png
[cache-model-add-class-dialog]: ./media/cache-web-app-howto/cache-model-add-class-dialog.png
[cache-add-controller]: ./media/cache-web-app-howto/cache-add-controller.png
[cache-add-controller-class]: ./media/cache-web-app-howto/cache-add-controller-class.png
[cache-configure-controller]: ./media/cache-web-app-howto/cache-configure-controller.png
[cache-global-asax]: ./media/cache-web-app-howto/cache-global-asax.png
[cache-RouteConfig-cs]: ./media/cache-web-app-howto/cache-RouteConfig-cs.png
[cache-layout-cshtml]: ./media/cache-web-app-howto/cache-layout-cshtml.png
[cache-layout-cshtml-code]: ./media/cache-web-app-howto/cache-layout-cshtml-code.png
[redis-cache-manage-nuget-menu]: ./media/cache-web-app-howto/redis-cache-manage-nuget-menu.png
[redis-cache-stack-exchange-nuget]: ./media/cache-web-app-howto/redis-cache-stack-exchange-nuget.png
[cache-teamscontroller]: ./media/cache-web-app-howto/cache-teamscontroller.png
[cache-web-config]: ./media/cache-web-app-howto/cache-web-config.png
[cache-views-teams-index-cshtml]: ./media/cache-web-app-howto/cache-views-teams-index-cshtml.png
[cache-teams-index-table]: ./media/cache-web-app-howto/cache-teams-index-table.png
[cache-status-message]: ./media/cache-web-app-howto/cache-status-message.png
[deploybutton]: ./media/cache-web-app-howto/deploybutton.png
[cache-deploy-to-azure-step-1]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-1.png
[cache-deploy-to-azure-step-2]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-2.png
[cache-deploy-to-azure-step-3]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-3.png
[cache-deployment-started]: ./media/cache-web-app-howto/cache-deployment-started.png
[cache-publish-app]: ./media/cache-web-app-howto/cache-publish-app.png
[cache-publish-to-app-service]: ./media/cache-web-app-howto/cache-publish-to-app-service.png
[cache-select-web-app]: ./media/cache-web-app-howto/cache-select-web-app.png
[cache-publish]: ./media/cache-web-app-howto/cache-publish.png
[cache-delete-resource-group]: ./media/cache-web-app-howto/cache-delete-resource-group.png
[cache-delete-confirm]: ./media/cache-web-app-howto/cache-delete-confirm.png


