---
title: Verwenden von Azure Redis Cache mit Node.js | Microsoft Docs
description: Erste Schritte mit Azure Redis Cache mit Node.js und &quot;node_redis&quot;.
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: v-lincan
ms.assetid: 06fddc95-8029-4a8d-83f5-ebd5016891d9
ms.service: cache
ms.devlang: nodejs
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 02/10/2017
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 5369dcd6ad1ec93c63eb442db9fc5ffdcca37375
ms.openlocfilehash: 530191637b1aa91ee1d7fe5b5bb032c60983f7dc


---
# <a name="how-to-use-azure-redis-cache-with-nodejs"></a>Verwenden von Azure Redis Cache mit Node.js
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

## <a name="prerequisites"></a>Voraussetzungen
Installieren Sie [node_redis](https://github.com/mranney/node_redis):

    npm install redis

In diesem Tutorial wird [node_redis](https://github.com/mranney/node_redis) verwendet. Beispiele für die Verwendung anderer Node.js-Clients finden Sie in der entsprechenden Dokumentation für die [hier](http://redis.io/clients#nodejs)aufgeführten Node.js-Clients.

## <a name="create-a-redis-cache-on-azure"></a>Erstellen eines Redis-Caches in Azure
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Abrufen des Hostnamens und der Zugriffsschlüssel
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a>Herstellen einer sicheren SSL-Verbindung mit dem Cache
Die neuesten Builds von [node_redis](https://github.com/mranney/node_redis) unterstützen das Herstellen einer SSL-Verbindung mit Azure Redis Cache. Das folgende Beispiel veranschaulicht das Herstellen einer Verbindung mit Azure Redis Cache unter Verwendung des SSL-Endpunkts 6380. Ersetzen Sie `<name>` durch den Namen Ihres Caches und `<key>` durch Ihren primären oder sekundären Schlüssel, wie im Abschnitt [Abrufen des Hostnamens und der Zugriffsschlüssel](#retrieve-the-host-name-and-access-keys) beschrieben.

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

> [!NOTE]
> Der Nicht-SSL-Port ist für neue Azure Redis Cache-Instanzen deaktiviert. Wenn Sie einen anderen Client verwenden, der SSL nicht unterstützt, lesen Sie die Informationen zum [Aktivieren des Nicht-SSL-Ports](cache-configure.md#access-ports).
> 
> 

## <a name="add-something-to-the-cache-and-retrieve-it"></a>Fügen Sie dem Cache Daten hinzu, und rufen Sie sie ab.
Das folgende Beispiel zeigt, wie Sie eine Verbindung mit einer Azure Redis Cache-Instanz herstellen, ein Element im Cache speichern und es aus dem Cache abrufen. Weitere Beispiele für die Verwendung von Redis mit dem [node_redis](https://github.com/mranney/node_redis)-Client finden Sie unter [http://redis.js.org/](http://redis.js.org/).

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


## <a name="next-steps"></a>Nächste Schritte
* [Aktivieren Sie die Cachediagnose](cache-how-to-monitor.md#enable-cache-diagnostics), damit Sie die Integrität Ihres Caches [überwachen](cache-how-to-monitor.md) können.
* Lesen Sie die offizielle [Redis-Dokumentation](http://redis.io/documentation).




<!--HONumber=Feb17_HO2-->


