---
title: "Cache – ASP.NET-Sitzungszustandsanbieter | Microsoft Docs"
description: Erfahren Sie, wie Sie den ASP.NET-Sitzungszustand mit Azure Redis Cache speichern.
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn
ms.assetid: 192f384c-836a-479a-bb65-8c3e6d6522bb
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 03/22/2017
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 1429bf0d06843da4743bd299e65ed2e818be199d
ms.openlocfilehash: 56a55bc10c9cf16751c713da302dcd59362ab80f
ms.lasthandoff: 03/22/2017


---
# <a name="aspnet-session-state-provider-for-azure-redis-cache"></a>ASP.NET-Sitzungszustandsanbieter für Azure Redis Cache
Azure Redis Cache stellt einen Sitzungszustandsanbieter bereit, mit dem Sie den Sitzungszustand in einem Cache anstatt im Arbeitsspeicher oder in einer SQL Server-Datenbank speichern können. Um den Sitzungszustandsanbieter zu verwenden, konfigurieren Sie zuerst den Cache, und konfigurieren Sie danach Ihre ASP.NET-Anwendung für den Cache mithilfe des Redis Cache-Sitzungszustands-NuGet-Pakets.

Im praktischen Einsatz von Cloud-Apps lässt sich das Speichern von Zustandsinformationen für eine Benutzersitzung oft nicht vermeiden. Allerdings beeinträchtigen einige Herangehensweisen die Leistung und Skalierbarkeit stärker als andere. Wenn Sie einen Sitzungszustand speichern müssen, ist es am besten, die Menge der Zustandsinformationen niedrig zu halten und sie in Cookies zu speichern. Wenn das nicht umsetzbar ist, besteht die nächstbeste Lösung darin, den ASP.NET-Sitzungszustand mit einem Anbieter für verteilten In-Memory-Cache zu verwenden. Die schlechteste Lösung im Hinblick auf Leistung und Skalierbarkeit wäre es, einen datenbankbasierten Sitzungszustandsanbieter zu verwenden. Dieses Thema bietet einen Leitfaden zum Verwenden des ASP.NET-Sitzungszustandsanbieters für Azure Redis Cache. Informationen zu anderen Optionen für den Sitzungszustand finden Sie unter [Optionen für den ASP.NET-Sitzungszustand](#aspnet-session-state-options).

## <a name="store-aspnet-session-state-in-the-cache"></a>Speichern des ASP.NET-Sitzungsstatus im Cache
Klicken Sie zum Konfigurieren einer Clientanwendung in Visual Studio mit dem Redis Cache-Sitzungszustands-NuGet-Paket im Menü **Extras** auf **NuGet-Paket-Manager** > **Paket-Manager-Konsole**.

Führen Sie im Fenster `Package Manager Console` den folgenden Befehl aus.
    
```
Install-Package Microsoft.Web.RedisSessionStateProvider
```

> [!IMPORTANT]
> Wenn Sie das Clusteringfeature aus dem Premium-Tarif verwenden, müssen Sie [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 oder höher verwenden. Andernfalls wird eine Ausnahme ausgelöst. Der Wechsel zu Version 2.0.1 oder einer höheren Version stellt eine grundlegende Änderung dar. Weitere Informationen finden Sie unter [Details zur grundlegenden Änderung von Version 2.0.0](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details). Zum Zeitpunkt dieser Artikelüberarbeitung ist die aktuelle Version dieses Pakets 2.2.3.
> 
> 

Das Redis-Sitzungszustandsanbieter-NuGet-Paket steht in einer Abhängigkeitsbeziehung zum Paket „StackExchange.Redis.StrongName“. Wenn das Paket „StackExchange.Redis.StrongName“ nicht in Ihrem Projekt vorhanden ist, wird es installiert.

>[!NOTE]
>Neben dem Paket „StackExchange.Redis.StrongName“ mit starkem Namen gibt es zudem noch das Paket „StackExchange.Redis“ als Version ohne starken Namen. Wenn in Ihrem Projekt die Version „StackExchange.Redis“ ohne starken Namen verwendet wird, müssen Sie sie deinstallieren. Andernfalls treten Namenskonflikte in Ihrem Projekt auf. Weitere Informationen über diese Pakete finden Sie unter [Konfigurieren der .NET-Cacheclients](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).
>
>

Mit dem heruntergeladenen NuGet-Paket werden die erforderlichen Assemblyverweise sowie der folgende Abschnitt in Ihrer Datei „web.config“ hinzugefügt. Dieser Abschnitt enthält die Konfiguration, die in Ihrer ASP.NET-Anwendung für die Verwendung des Redis Cache-Sitzungszustandsanbieters erforderlich ist.

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
    <!--
    <add name="MySessionStateStore"
           host = "127.0.0.1" [String]
        port = "" [number]
        accessKey = "" [String]
        ssl = "false" [true|false]
        throwOnError = "true" [true|false]
        retryTimeoutInMilliseconds = "0" [number]
        databaseId = "0" [number]
        applicationName = "" [String]
        connectionTimeoutInMilliseconds = "5000" [number]
        operationTimeoutInMilliseconds = "5000" [number]
    />
    -->
    <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false"/>
    </providers>
</sessionState>
```

Im kommentierten Bereich finden Sie Beispiele der Attribute sowie Beispieleinstellungen für jedes Attribut.

Konfigurieren Sie die Attribute mit den Werten vom Blatt „Cache“ im Microsoft Azure-Portal, und konfigurieren Sie die restlichen Werte wie gewünscht. Anweisungen zum Zugreifen auf die Cacheeigenschaften finden Sie unter [Konfigurieren von Redis-Cacheeinstellungen](cache-configure.md#configure-redis-cache-settings).

* **host** : Geben Sie den Cacheendpunkt an.
* **port** : Verwenden Sie abhängig von Ihren SSL-Einstellungen entweder den Nicht-SSL-Port oder den SSL-Port.
* **accessKey** : Verwenden Sie den primären oder sekundären Schlüssel für Ihren Cache.
* **ssl** : Geben Sie „true“ an, wenn Sie die Kommunikation zwischen Cache und Client mit SSL absichern möchten, andernfalls „false“. Achten Sie darauf, dass Sie den richtigen Port angeben.
  * Der Nicht-SSL-Port ist für neue Caches standardmäßig deaktiviert. Geben Sie für diese Einstellung „true“ an, wenn Sie den SSL-Port verwenden möchten. Weitere Informationen zum Aktivieren des Nicht-SSL-Ports finden Sie im Abschnitt [Zugriffsports](cache-configure.md#access-ports) des Themas [Konfigurieren eines Caches](cache-configure.md).
* **throwOnError:** Geben Sie TRUE an, wenn bei einem Fehler eine Ausnahme ausgelöst werden soll, oder FALSE, wenn bei einem nicht ausgeführten Vorgang keine Meldung erfolgen soll. Sie können das Vorliegen von Fehlern feststellen, indem Sie die statische Eigenschaft „Microsoft.Web.Redis.RedisSessionStateProvider.LastException“ überprüfen. Der Standardwert ist „true“.
* **retryTimeoutInMilliseconds**: Bei Fehlern werden Vorgänge für diese Zeitspanne wiederholt (angegeben in Millisekunden). Die erste Wiederholung erfolgt nach 20 Millisekunden. Danach wird die Wiederholung jede Sekunde durchgeführt, bis die in „retryTimeoutInMilliseconds“ angegebene Zeitspanne abgelaufen ist. Unmittelbar nach dieser Zeitspanne wird der Vorgang ein letztes Mal wiederholt. Wenn dann immer noch ein Fehler bei dem Vorgang auftritt, wird der aufrufenden Funktion eine Ausnahme zurückgegeben, abhängig von der Einstellung für „throwOnError“. Der Standardwert lautet 0, d.h. keine Wiederholungen.
* **databaseId** : Gibt an, welche Datenbank für die Cacheausgabedaten verwendet werden soll. Wenn Sie hier nichts angeben, wird der Standardwert 0 verwendet.
* **applicationName**: Schlüssel werden in Redis als `{<Application Name>_<Session ID>}_Data` gespeichert. Durch dieses Benennungsschema wird ermöglicht, dass mehrere Anwendungen denselben Schlüssel gemeinsam nutzen. Dieser Parameter ist optional, und wenn Sie ihn nicht angeben, wird der Standardwert verwendet.
* **connectionTimeoutInMilliseconds** : Diese Einstellung ermöglicht Ihnen, die connectTimeout-Einstellung im StackExchange.Redis-Client zu überschreiben. Wenn Sie sie nicht angegeben, wird die connectTimeout-Standardeinstellung „5000“ verwendet. Weitere Informationen finden Sie unter [StackExchange.Redis-Konfigurationsmodell](http://go.microsoft.com/fwlink/?LinkId=398705).
* **operationTimeoutInMilliseconds** : Diese Einstellung ermöglicht Ihnen, die syncTimeout-Einstellung im StackExchange.Redis-Client zu überschreiben. Wenn Sie sie nicht angegeben, wird die syncTimeout-Standardeinstellung „1000“ verwendet. Weitere Informationen finden Sie unter [StackExchange.Redis-Konfigurationsmodell](http://go.microsoft.com/fwlink/?LinkId=398705).

Weitere Informationen über diese Eigenschaften finden Sie im Blogbeitrag mit der ursprünglichen Ankündigung: [Announcing ASP.NET Session State Provider for Redis](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)(in englischer Sprache).

Vergessen Sie nicht, den Abschnitt mit dem als Standard verwendeten InProc-Sitzungszustandsanbieter in Ihrer web.config-Datei auszukommentieren.

```xml
<!-- <sessionState mode="InProc"
     customProvider="DefaultSessionProvider">
     <providers>
        <add name="DefaultSessionProvider"
              type="System.Web.Providers.DefaultSessionStateProvider,
                    System.Web.Providers, Version=1.0.0.0, Culture=neutral,
                    PublicKeyToken=31bf3856ad364e35"
              connectionStringName="DefaultConnection" />
      </providers>
</sessionState> -->
```

Nach Abschluss dieser Schritte ist Ihre Anwendung für die Verwendung des Redis Cache-Sitzungszustandsanbieters konfiguriert. Wenn Sie den Sitzungszustand in Ihrer Anwendung verwenden, wird er in einer Azure Redis Cache-Instanz gespeichert.

> [!IMPORTANT]
> Die im Cache gespeicherten Daten müssen serialisierbar sein. Sie unterscheiden sich darin von den Daten, die im standardmäßigen In-Memory-ASP.NET-Sitzungszustandsanbieter gespeichert werden können. Wenn der Sitzungszustandsanbieter für Redis verwendet wird, müssen Sie darauf achten, dass die im Sitzungszustand gespeicherten Datentypen serialisierbar sind.
> 
> 

## <a name="aspnet-session-state-options"></a>Optionen für den ASP.NET-Sitzungszustand
* In-Memory-Sitzungszustandsanbieter: Dieser Anbieter speichert den Sitzungszustand im Arbeitsspeicher. Der Vorteil bei diesem Anbieter ist, dass er einfach und schnell ist. Wenn Sie einen In-Memory-Anbieter verwenden, können Sie jedoch Ihre Web-Apps nicht skalieren, denn dieser Anbieter ist nicht verteilt.
* SQL Server-Sitzungszustandsanbieter: Dieser Anbieter speichert den Sitzungszustand in SQL Server. Verwenden Sie diesen Anbieter, wenn der Sitzungszustand in einem beständigen Speicher gespeichert werden soll. Sie können Ihre Web-App skalieren, die Verwendung von SQL Server für den Sitzungszustand hat jedoch Auswirkungen auf die Leistung der Web-App.
* Verteilter In-Memory-Sitzungszustandsanbieter wie z. B. Redis Cache-Sitzungszustandsanbieter: Dieser Anbieter bietet Ihnen das Beste aus beiden Welten. Ihre Web-App kann über einen einfachen, schnellen und skalierbaren Sitzungszustandsanbieter verfügen. Da dieser Anbieter den Sitzungszustand in einem Cache speichert, muss Ihre App so ausgelegt sein, dass die besonderen Merkmale bei der Kommunikation mit einem verteilten In-Memory-Cache berücksichtigt werden, z.B. kurze Netzwerkausfälle. Bewährte Verfahren zum Verwenden des Caches finden Sie unter [Caching Guidance](../best-practices-caching.md) (in englischer Sprache, aus Microsoft Patterns & Practices, [Azure Cloud Application Design and Implementation Guidance](https://github.com/mspnp/azure-guidance)).

Weitere Information über das Thema Sitzungszustand und andere bewährte Verfahren finden Sie unter [Web Development Best Practices (Building Real-World Cloud Apps with Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices)(in englischer Sprache).

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie [ASP.NET-Ausgabecacheanbieter für Azure Redis Cache](cache-aspnet-output-cache-provider.md).


