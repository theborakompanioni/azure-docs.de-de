---
title: Verwenden von Azure Redis Cache mit Java | Microsoft Docs
description: Erste Schritte mit Azure Redis Cache mit Java
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 29275a5e-2e39-4ef2-804f-7ecc5161eab9
ms.service: cache
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 02/10/2017
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 5369dcd6ad1ec93c63eb442db9fc5ffdcca37375
ms.openlocfilehash: b95f37db90b105962c01545e25c8e14c53257ebc


---
# <a name="how-to-use-azure-redis-cache-with-java"></a>Verwenden von Azure Redis Cache mit Java
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Azure Redis Cache bietet Zugriff auf einen dedizierten Redis-Cache, der von Microsoft verwaltet wird. Auf Ihren Cache kann in jeder Anwendung in Microsoft Azure zugegriffen werden.

Dieses Thema beschreibt die ersten Schritte mit Azure Redis Cache und Java.

## <a name="prerequisites"></a>Voraussetzungen
[Jedis](https://github.com/xetorthio/jedis) – Java-Client für Redis

In diesem Tutorial wird Jedis verwendet. Sie können jedoch alle unter [http://redis.io/clients](http://redis.io/clients) aufgeführten Java-Clients nutzen.

## <a name="create-a-redis-cache-on-azure"></a>Erstellen eines Redis-Caches in Azure
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Abrufen des Hostnamens und der Zugriffsschlüssel
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a>Herstellen einer sicheren SSL-Verbindung mit dem Cache
Die neuesten Builds von [jedis](https://github.com/xetorthio/jedis) unterstützen das Herstellen einer SSL-Verbindung mit Azure Redis Cache. Das folgende Beispiel veranschaulicht das Herstellen einer Verbindung mit Azure Redis Cache unter Verwendung des SSL-Endpunkts 6380. Ersetzen Sie `<name>` durch den Namen Ihres Caches und `<key>` durch Ihren primären oder sekundären Schlüssel, wie im Abschnitt [Abrufen des Hostnamens und der Zugriffsschlüssel](#retrieve-the-host-name-and-access-keys) beschrieben.

    boolean useSsl = true;
    /* In this line, replace <name> with your cache name: */
    JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379, useSsl);
    shardInfo.setPassword("<key>"); /* Use your access key. */

> [!NOTE]
> Der Nicht-SSL-Port ist für neue Azure Redis Cache-Instanzen deaktiviert. Wenn Sie einen anderen Client verwenden, der SSL nicht unterstützt, lesen Sie die Informationen zum [Aktivieren des Nicht-SSL-Ports](cache-configure.md#access-ports).
> 
> 

## <a name="add-something-to-the-cache-and-retrieve-it"></a>Fügen Sie dem Cache Daten hinzu, und rufen Sie sie ab.
    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    public class App
    {
      public static void main( String[] args )
      {
        boolean useSsl = true;
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379, useSsl);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## <a name="next-steps"></a>Nächste Schritte
* [Aktivieren Sie die Cachediagnose](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics), damit Sie die Integrität Ihres Caches [überwachen](https://msdn.microsoft.com/library/azure/dn763945.aspx) können.
* Lesen Sie die offizielle [Redis-Dokumentation](http://redis.io/documentation).



<!--HONumber=Feb17_HO2-->


