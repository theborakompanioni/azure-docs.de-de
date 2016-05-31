<properties
	pageTitle="Verwenden von Azure Redis Cache mit Node.js | Microsoft Azure"
	description="Erste Schritte mit Azure Redis Cache mit Node.js und ";node_redis";."
	services="redis-cache"
	documentationCenter=""
	authors="steved0x"
	manager="erikre"
	editor="v-lincan"/>

<tags
	ms.service="cache"
	ms.devlang="nodejs"
	ms.topic="hero-article"
	ms.tgt_pltfrm="cache-redis"
	ms.workload="tbd"
	ms.date="05/24/2016"
	ms.author="sdanie"/>

# Verwenden von Azure Redis Cache mit Node.js

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure Redis Cache bietet Zugriff auf einen sicheren dedizierten Redis-Cache, der von Microsoft verwaltet wird. Auf Ihren Cache kann in jeder Anwendung in Microsoft Azure zugegriffen werden.

Dieses Thema beschreibt die ersten Schritte mit Azure Redis Cache und Node.js. Ein anderes Beispiel zum Verwenden von Azure Redis Cache mit Node.js finden Sie unter [Erstellen einer Node.js-Chatanwendung mit Socket.IO auf einer Azure-Website](../app-service-web/web-sites-nodejs-chat-app-socketio.md).


## Voraussetzungen

Installieren Sie [node\_redis](https://github.com/mranney/node_redis):

    npm install redis

In diesem Lernprogramm wird [node\_redis](https://github.com/mranney/node_redis) verwendet. Sie können jedoch alle unter [http://redis.io/clients](http://redis.io/clients) aufgeführten Node.js-Clients verwenden.

## Erstellen eines Redis-Caches in Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## Abrufen des Hostnamens und der Zugriffsschlüssel

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## Aktivieren des Nicht-SSL-Endpunkts

Einige Redis-Clients weisen keine Unterstützung für SSL auf, und der [Nicht-SSL-Port ist für neue Azure Redis Cache-Instanzen standardmäßig deaktiviert](cache-configure.md#access-ports). Zum Zeitpunkt der Erstellung dieses Dokuments wird SSL vom [node\_redis](https://github.com/mranney/node_redis)-Client nicht unterstützt.

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]


## Fügen Sie dem Cache Daten hinzu, und rufen Sie sie ab.

	  var redis = require("redis");
	
	  // Add your cache name and access key.
	var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});
	
	client.set("key1", "value", function(err, reply) {
		    console.log(reply);
		});
	
	client.get("key1",  function(err, reply) {
		    console.log(reply);
		});

Ausgabe:

	OK
	value


## Nächste Schritte

- [Aktivieren Sie die Cachediagnose](cache-how-to-monitor.md#enable-cache-diagnostics), damit Sie die Integrität Ihres Caches [überwachen](cache-how-to-monitor.md) können.
- Lesen Sie die offizielle [Redis-Dokumentation](http://redis.io/documentation).

<!---HONumber=AcomDC_0525_2016-->