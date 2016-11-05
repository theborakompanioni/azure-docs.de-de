---
title: Verwenden von Azure Redis Cache mit Node.js | Microsoft Docs
description: 'Erste Schritte mit Azure Redis Cache mit Node.js und '
;node_redis";.": ''
services: redis-cache
documentationcenter: ''
author: steved0x
manager: douge
editor: v-lincan

ms.service: cache
ms.devlang: nodejs
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 08/16/2016
ms.author: sdanie

---
# Verwenden von Azure Redis Cache mit Node.js
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Azure Redis Cache bietet Zugriff auf einen sicheren dedizierten Redis-Cache, der von Microsoft verwaltet wird. Auf Ihren Cache kann in jeder Anwendung in Microsoft Azure zugegriffen werden.

Dieses Thema beschreibt die ersten Schritte mit Azure Redis Cache und Node.js. Ein anderes Beispiel zum Verwenden von Azure Redis Cache mit Node.js finden Sie unter [Erstellen einer Node.js-Chatanwendung mit Socket.IO auf einer Azure-Website](../app-service-web/web-sites-nodejs-chat-app-socketio.md).

## Voraussetzungen
Installieren Sie [node\_redis](https://github.com/mranney/node_redis):

    npm install redis

In diesem Tutorial wird [node\_redis](https://github.com/mranney/node_redis) verwendet. Beispiele für die Verwendung anderer Node.js-Clients finden Sie in der entsprechenden Dokumentation für die [hier](http://redis.io/clients#nodejs) aufgeführten Node.js-Clients.

## Erstellen eines Redis-Caches in Azure
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## Abrufen des Hostnamens und der Zugriffsschlüssel
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## Herstellen einer sicheren SSL-Verbindung mit dem Cache
Die neuesten Builds von [node\_redis](https://github.com/mranney/node_redis) unterstützen das Herstellen einer SSL-Verbindung mit Azure Redis Cache. Das folgende Beispiel veranschaulicht das Herstellen einer Verbindung mit Azure Redis Cache unter Verwendung des SSL-Endpunkts 6380. Ersetzen Sie `<name>` durch den Namen Ihres Caches und `<key>` durch Ihren primären oder sekundären Schlüssel, wie im Abschnitt [Abrufen des Hostnamens und der Zugriffsschlüssel](#retrieve-the-host-name-and-access-keys) beschrieben.

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});


## Fügen Sie dem Cache Daten hinzu, und rufen Sie sie ab.
Das folgende Beispiel zeigt, wie Sie eine Verbindung mit einer Azure Redis Cache-Instanz herstellen, ein Element im Cache speichern und es aus dem Cache abrufen. Weitere Beispiele für die Verwendung von Redis mit dem [node\_redis](https://github.com/mranney/node_redis)-Client finden Sie unter [http://redis.js.org/](http://redis.js.org/).

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
* [Aktivieren Sie die Cachediagnose](cache-how-to-monitor.md#enable-cache-diagnostics), damit Sie die Integrität Ihres Caches [überwachen](cache-how-to-monitor.md) können.
* Lesen Sie die offizielle [Redis-Dokumentation](http://redis.io/documentation).

<!---HONumber=AcomDC_0817_2016-->