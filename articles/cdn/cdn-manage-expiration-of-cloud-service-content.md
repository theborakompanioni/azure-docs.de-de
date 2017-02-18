---
title: Verwalten des Ablaufs von Webinhalten in Azure CDN | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Ablauf von Inhalten aus Azure-Web-Apps/Cloud Services, ASP.NET oder IIS in Azure CDN verwalten.
services: cdn
documentationcenter: .NET
author: zhangmanling
manager: erikre
editor: 
ms.assetid: bef53fcc-bb13-4002-9324-9edee9da8288
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
translationtype: Human Translation
ms.sourcegitcommit: 57d00f2192fed7a2e89ac94e110ebb7e84c83b72
ms.openlocfilehash: 02d0270c5763eb9dd2190bc24b793022ea536746


---
# <a name="manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a>Verwalten des Ablaufs von Inhalten aus Azure-Web-Apps/Cloud Services, ASP.NET oder IIS in Azure CDN
> [!div class="op_single_selector"]
> * [Azure-Web-Apps/Cloud Services, ASP.NET oder IIS](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Storage Blob-Dienst](cdn-manage-expiration-of-blob-content.md)
> 
> 

Dateien von öffentlich zugänglichen Ursprungswebservern können bis zum Ende ihrer Gültigkeitsdauer in Azure CDN zwischengespeichert werden.  Die Gültigkeitsdauer wird durch den [*Cache-Control*-Header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in der HTTP-Antwort des Ursprungsservers bestimmt.  In diesem Artikel erfahren Sie, wie Sie `Cache-Control`-Header für Azure-Web-Apps, Azure Cloud Services, ASP.NET-Anwendungen und Internetinformationsdienste-Websites festlegen. Die Konfiguration ist dabei jeweils ähnlich.

> [!TIP]
> Sie haben auch die Möglichkeit, keine Gültigkeitsdauer für eine Datei festzulegen.  In diesem Fall wendet Azure CDN automatisch eine Standardgültigkeitsdauer von sieben Tagen an.
> 
> Weitere Informationen dazu, wie Azure CDN den Zugriff auf Dateien und andere Ressourcen beschleunigen kann, finden Sie in der [Übersicht über das Azure Content Delivery Network (CDN)](cdn-overview.md).
> 
> 

## <a name="setting-cache-control-headers-in-configuration"></a>Festlegen von Cache-Control-Headern in der Konfiguration
Bei statischen Inhalten wie Bildern und Stylesheets können Sie das Aktualisierungsintervall durch Ändern der Datei **applicationHost.config** oder **web.config** für Ihre Webanwendung steuern.  Das Element **system.webServer\staticContent\clientCache** in der Konfigurationsdatei legt den `Cache-Control`-Header für Ihre Inhalte fest. Für **web.config**gilt: Die Konfigurationseinstellungen wirken sich auf alle Inhalte des Ordners sowie der Unterordner aus, solange dies nicht auf Unterordnerebene überschrieben wird.  Beispielsweise können Sie auf der Stammebene eine standardmäßige Gültigkeitsdauer festlegen, damit alle statischen Inhalte 3 Tage zwischengespeichert werden, während in einem Unterordner mit stärker veränderlichem Inhalt eine Cacheeinstellung von 6 Stunden verwendet wird.  **applicationHost.config** betrifft alle Anwendungen auf der Website, kann jedoch mithilfe von Dateien vom Typ **web.config** in den Anwendungen überschrieben werden.

Die folgende XML zeigt ein Beispiel zum Festlegen von **clientCache** auf ein maximales Alter von 3 Tagen:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Durch Angeben von **UseMaxAge** wird der Antwort auf der Grundlage des im **cacheControlMaxAge**-Attribut angegebenen Werts ein Header vom Typ `Cache-Control: max-age=<nnn>` hinzugefügt. Das Format des Zeitraums für das **cacheControlMaxAge**-Attribut lautet <days>.<hours>:<min>:<sec>. Weitere Informationen zum **clientCache**-Knoten finden Sie unter [Clientcache<clientCache>](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>Festlegen von Cache-Control-Headern im Code
Bei ASP.NET-Anwendungen können Sie das Verhalten von CDN beim Zwischenspeichern mithilfe der **HttpResponse.Cache** -Eigenschaft programmgesteuert festlegen. Weitere Informationen zur **HttpResponse.Cache**-Eigenschaft finden Sie unter [HttpResponse.CacheEigenschaft](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) und [HttpCachePolicy-Klasse](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Wenn Sie Anwendungsinhalte in ASP.NET programmgesteuert zwischenspeichern möchten, stellen Sie sicher, dass der Inhalt entsprechend markiert ist, indem Sie „HttpCacheability“ auf *Public* festlegen. Stellen Sie außerdem sicher, dass eine Cachevalidierung durchgeführt werden kann. Zu diesem Zweck kann ein Zeitstempel der letzten Änderung festgelegt werden, indem "SetLastModified" aufgerufen wird. Sie können auch einen "etag"-Wert festlegen, indem Sie "SetETag" aufrufen. Optional können Sie eine Ablaufzeit für den Cache angeben, indem Sie "SetExpires" aufrufen, oder Sie können sich auf die standardmäßige Cacheheuristik verlassen, die weiter oben in diesem Dokument beschrieben wird.  

Fügen Sie beispielsweise folgenden Code hinzu, um Inhalt eine Stunde lang zwischenzuspeichern:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a>Nächste Schritte
* [Lesen Sie ausführliche Informationen zum **clientCache**-Element.](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [Lesen Sie die Dokumentation für die **HttpResponse.Cache**-Eigenschaft.](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
* [Lesen Sie die Dokumentation für die **HttpCachePolicy**-Klasse](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  




<!--HONumber=Jan17_HO4-->


