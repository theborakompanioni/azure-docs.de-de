<properties
	pageTitle="Verwenden von Azure Redis Cache mit Python | Microsoft Azure"
	description="Erste Schritte mit Azure Redis Cache und Python"
	services="redis-cache"
	documentationCenter=""
	authors="steved0x"
	manager="douge"
	editor="v-lincan"/>

<tags
	ms.service="cache"
	ms.devlang="python"
	ms.topic="hero-article"
	ms.tgt_pltfrm="cache-redis"
	ms.workload="tbd"
	ms.date="05/31/2016"
	ms.author="sdanie"/>

# Verwenden von Azure Redis Cache mit Python

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Dieses Thema beschreibt die ersten Schritte mit Azure Redis Cache und Python.


## Voraussetzungen

Installieren Sie [redis-py](https://github.com/andymccurdy/redis-py).


## Erstellen eines Redis-Caches in Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## Abrufen des Hostnamens und der Zugriffsschlüssel

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## Aktivieren des Nicht-SSL-Endpunkts

Einige Redis-Clients weisen keine Unterstützung für SSL auf, und der [Nicht-SSL-Port ist für neue Azure Redis Cache-Instanzen standardmäßig deaktiviert](cache-configure.md#access-ports). Zum Zeitpunkt der Erstellung dieses Dokuments wird SSL vom [redis-py](https://github.com/andymccurdy/redis-py)-Client nicht unterstützt.

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]


## Fügen Sie dem Cache Daten hinzu, und rufen Sie sie ab.


	>>> import redis
	>>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
	      port=6380, db=0, password='<key>', ssl=True)
	>>> r.set('foo', 'bar')
	True
	>>> r.get('foo')
	b'bar'


Ersetzen Sie `<name>` durch den Namen Ihres Caches und `key` durch Ihren Zugriffsschlüssel.


<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png

<!---HONumber=AcomDC_0601_2016-->