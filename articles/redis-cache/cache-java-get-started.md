<properties
   pageTitle="Verwenden von Azure Redis Cache mit Java | Microsoft Azure"
	description="Erste Schritte mit Azure Redis Cache mit Java"
	services="redis-cache"
	documentationCenter=""
	authors="steved0x"
	manager="douge"
	editor=""/>

<tags
	ms.service="cache"
	ms.devlang="java"
	ms.topic="hero-article"
	ms.tgt_pltfrm="cache-redis"
	ms.workload="tbd"
	ms.date="05/31/2016"
	ms.author="sdanie"/>

# Verwenden von Azure Redis Cache mit Java

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure Redis Cache bietet Zugriff auf einen dedizierten Redis-Cache, der von Microsoft verwaltet wird. Auf Ihren Cache kann in jeder Anwendung in Microsoft Azure zugegriffen werden.

Dieses Thema beschreibt die ersten Schritte mit Azure Redis Cache und Java.

## Voraussetzungen

[Jedis](https://github.com/xetorthio/jedis) – Java-Client für Redis

In diesem Lernprogramm wird Jedis verwendet. Sie können jedoch alle unter [http://redis.io/clients](http://redis.io/clients) aufgeführten Java-Clients nutzen.

## Erstellen eines Redis-Caches in Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## Abrufen des Hostnamens und der Zugriffsschlüssel

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## Aktivieren des Nicht-SSL-Endpunkts

Einige Redis-Clients weisen keine Unterstützung für SSL auf, und der [Nicht-SSL-Port ist für neue Azure Redis Cache-Instanzen standardmäßig deaktiviert](cache-configure.md#access-ports). Zum Zeitpunkt der Erstellung dieses Dokuments wird SSL vom [Jedis](https://github.com/xetorthio/jedis)-Client nicht unterstützt.

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]




## Fügen Sie dem Cache Daten hinzu, und rufen Sie sie ab.

	package com.mycompany.app;
	import redis.clients.jedis.Jedis;
	import redis.clients.jedis.JedisShardInfo;

	/* Make sure you turn on non-SSL port in Azure Redis using the Configuration section in the Azure Portal */
	public class App
	{
	  public static void main( String[] args )
	  {
        /* In this line, replace <name> with your cache name: */
	    JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379);
	    shardInfo.setPassword("<key>"); /* Use your access key. */
	    Jedis jedis = new Jedis(shardInfo);
     	jedis.set("foo", "bar");
     	String value = jedis.get("foo");
	  }
	}


## Nächste Schritte

- [Aktivieren Sie die Cachediagnose](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics), damit Sie die Integrität Ihres Caches [überwachen](https://msdn.microsoft.com/library/azure/dn763945.aspx) können.
- Lesen Sie die offizielle [Redis-Dokumentation](http://redis.io/documentation).

<!---HONumber=AcomDC_0601_2016-->