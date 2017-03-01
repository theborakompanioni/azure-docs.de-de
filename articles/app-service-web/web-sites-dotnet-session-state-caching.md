---
title: Sitzungsstatus mit Azure Redis Cache in Azure App Service
description: "Erfahren Sie, wie Sie den Azure Cache Service zur Unterstützung des ASP.NET-Sitzungszustand-Caching verwenden."
services: app-service\web
documentationcenter: .net
author: Rick-Anderson
manager: erikre
editor: none
ms.assetid: 4f98d289-2698-464d-85cd-99ec40fad1f2
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 06/27/2016
ms.author: riande
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: a682e51bfaed9056b170c3e9473180ca210557b9
ms.lasthandoff: 01/20/2017


---
# <a name="session-state-with-azure-redis-cache-in-azure-app-service"></a>Sitzungsstatus mit Azure Redis Cache in Azure App Service
In diesem Thema wird erläutert, wie Sie den Azure Redis Cache-Dienst für den Sitzungszustand verwenden.

Falls Ihre ASP.NET-Web-App den Sitzungszustand verwendet, müssen Sie einen externen Sitzungszustandsanbieter (entweder den Redis Cache Service oder einen SQL Server-Sitzungszustandsanbieter) konfigurieren. Wird der Sitzungszustand ohne einen externen Anbieter verwendet, sind Sie auf eine Instanz Ihrer Web-App beschränkt. Der Redis Cache Service lässt sich am schnellsten und einfachsten aktivieren.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="a-idcreatecacheacreate-the-cache"></a><a id="createcache"></a>Erstellen des Caches
Befolgen Sie [diese Anweisungen](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) zum Erstellen des Caches.

## <a name="a-idconfigureprojectaadd-the-redissessionstateprovider-nuget-package-to-your-web-app"></a><a id="configureproject"></a>Hinzufügen des RedisSessionStateProvider-NuGet-Pakets zur Web-App
Installieren Sie das `RedisSessionStateProvider` -NuGet-Paket.  Verwenden Sie den folgenden Befehl zur Installation über die Paket-Manager-Konsole (**Extras** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole**):

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`

Bei Installation über **Extras** > **NuGet-Paket-Manager** > **NuGet-Pakete für Projektmappe verwalten** suchen Sie nach `RedisSessionStateProvider`.

Weitere Informationen finden Sie auf der [NuGet-RedisSessionStateProvider-Seite](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/) und unter [Konfigurieren der Cacheclients](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).

## <a name="a-idconfigurewebconfigamodify-the-webconfig-file"></a><a id="configurewebconfig"></a>Ändern der Datei "web.config"
Das NuGet-Paket fügt nicht nur Assemblyverweise für Cache, sondern auch Stub-Einträge in die Datei *web.config* ein. 

1. Öffnen Sie *web.config* , und suchen Sie das Element **sessionState** .
2. Geben Sie die Werte für `host`, `accessKey`, `port` (der SSL-Port sollte 6380 lauten) ein, und legen Sie `SSL` auf `true` fest. Diese Werte finden Sie im [Azure-Portal](http://go.microsoft.com/fwlink/?LinkId=529715) auf dem Blatt für Ihre Cacheinstanz. Weitere Informationen finden Sie unter [Verbinden mit dem Cache](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache). Beachten Sie, dass der Nicht-SSL-Port für neue Caches standardmäßig deaktiviert ist. Weitere Informationen zum Aktivieren des Nicht-SSL-Ports finden Sie im Abschnitt [Zugriffsports](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) des Themas [Konfigurieren eines Caches in Azure Redis Cache](https://msdn.microsoft.com/library/azure/dn793612.aspx). Das folgende Markup zeigt die Änderungen an der Datei *web.config*, insbesondere die Änderungen an *port*, *host*, accessKey* und *ssl*.
   
          <system.web>;
            <customErrors mode="Off" />;
            <authentication mode="None" />;
            <compilation debug="true" targetFramework="4.5" />;
            <httpRuntime targetFramework="4.5" />;
            <sessionState mode="Custom" customProvider="RedisSessionProvider">;
              <providers>;  
                  <!--<add name="RedisSessionProvider" 
                    host = "127.0.0.1" [String]
                    port = "" [number]
                    accessKey = "" [String]
                    ssl = "false" [true|false]
                    throwOnError = "true" [true|false]
                    retryTimeoutInMilliseconds = "0" [number]
                    databaseId = "0" [number]
                    applicationName = "" [String]
                  />;-->;
                 <add name="RedisSessionProvider" 
                      type="Microsoft.Web.Redis.RedisSessionStateProvider" 
                      port="6380"
                      host="movie2.redis.cache.windows.net" 
                      accessKey="m7PNV60CrvKpLqMUxosC3dSe6kx9nQ6jP5del8TmADk=" 
                      ssl="true" />;
              <!--<add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false" />;-->;
              </providers>;
            </sessionState>;
          </system.web>;

## <a name="a-idusesessionobjecta-use-the-session-object-in-code"></a><a id="usesessionobject"></a> Verwenden des Sitzungsobjekts im Code
Der letzte Schritt ist die Verwendung des Sitzungsobjekts im ASP.NET-Code. Fügen Sie Objekte mit der Methode **Session.Add** zum Sitzungsstatus hinzu. Diese Methode verwendet Schlüssel-Wert-Paare, um Elemente im Sitzungszustandscache zu speichern.

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

Der folgende Code ruft diesen Wert aus dem Sitzungszustand ab.

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue;    

Sie können den Redis Cache auch zum Zwischenspeichern von Objekten in Ihrer Web-App verwenden. Weitere Informationen finden Sie unter [MVC-Film-App mit Azure Redis Cache in 15 Minuten](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).
Weitere Informationen zur Verwendung des ASP.NET-Sitzungsstatus finden Sie unter [Überblick über den ASP.NET-Sitzungsstatus][ASP.NET Session State Overview].

> [!NOTE]
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](https://azure.microsoft.com/try/app-service/)sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.
> 
> 

## <a name="whats-changed"></a>Änderungen
* Hinweise zu den Änderungen von Websites zum App Service finden Sie unter: [Azure App Service und vorhandene Azure-Dienste](http://go.microsoft.com/fwlink/?LinkId=529714)
  
  *Von [Rick Anderson](https://twitter.com/RickAndMSFT)*

[installed the latest]: http://www.windowsazure.com/downloads/?sdk=net  
[ASP.NET Session State Overview]: http://msdn.microsoft.com/library/ms178581.aspx

[NewIcon]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
[NewCacheDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
[CacheIcon]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
[NuGetDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
[OutputConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
[CacheConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
[EndpointURL]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
[ManageKeys]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png


