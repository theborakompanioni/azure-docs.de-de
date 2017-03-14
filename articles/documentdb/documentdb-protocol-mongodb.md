---
title: "Was ist DocumentDB: API für MongoDB? | Microsoft Docs"
description: "Weitere Informationen zu DocumentDB: API für MongoDB und wie Sie vorhandene MongoDB-Anwendungen problemlos in der Azure Cloud ausführen können"
keywords: what is MongoDB
services: documentdb
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 4afaf40d-c560-42e0-83b4-a64d94671f0a
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: anhoh
translationtype: Human Translation
ms.sourcegitcommit: 72b2d9142479f9ba0380c5bd2dd82734e370dee7
ms.openlocfilehash: 6c143a18883c99a24264d3174df7ec214d696fb5
ms.lasthandoff: 03/08/2017


---
# <a name="what-is-documentdb-api-for-mongodb"></a>Was ist DocumentDB: API für MongoDB?

DocumentDB-Datenbanken können jetzt als Datenspeicher für Apps verwendet werden, die für MongoDB geschrieben wurden. Dies bedeutet, dass Ihre für MongoDB geschriebene Anwendung durch Nutzung vorhandener [Treiber](https://docs.mongodb.org/ecosystem/drivers/) für MongoDB-Datenbanken jetzt mit DocumentDB kommunizieren und DocumentDB-Datenbanken statt MongoDB-Datenbanken verwenden kann. In vielen Fällen können Sie von der Verwendung von MongoDB zu DocumentDB wechseln, indem Sie einfach eine Verbindungszeichenfolge ändern. Mit dieser Funktion können Kunden – unter Verwendung der vollständig verwalteten und skalierbaren NoSQL-Datenbanken von DocumentDB – ganz einfach MongoDB-Datenbankanwendungen in der Azure-Cloud erstellen und ausführen und weiterhin die vorhandenen Kenntnisse und Tools für MongoDB verwenden.

## <a name="what-is-the-benefit-of-using-documentdb-api-for-mongodb"></a>Welche Vorteile bietet die Verwendung von DocumentDB: API für MongoDB?
**Keine Serververwaltung**: DocumentDB ist ein vollständig verwalteter Dienst, d.h. Sie müssen selbst keine Infrastruktur oder virtuellen Computer verwalten. DocumentDB ist in mehr als 20 [Azure-Regionen](https://azure.microsoft.com/regions/services/) verfügbar.

**Unbegrenzte Skalierung**: Sie können Durchsatz und Speicherressourcen unabhängig und elastisch skalieren. Sie können weitere Kapazität hinzufügen, um problemlos Millionen von Anforderungen pro Sekunde zu verarbeiten.

**Unternehmensniveau**: DocumentDB unterstützt mehrere lokale Replikate, um eine Verfügbarkeit von 99,99 % sowie Datenschutz bei lokalen und regionalen Ausfällen bereitzustellen. DocumentDB verfügt über [Kompatibilitätszertifizierungen](https://www.microsoft.com/trustcenter) und Sicherheitsfunktionen auf Unternehmensniveau. 

**Kompatibilität mit MongoDB**: DocumentDB: API für MongoDB wurde für Kompatibilität mit MongoDB konzipiert. Für die Arbeit mit DocumentDB können Sie Ihre vorhandenen Codezeilen, Anwendungen, Treiber und Tools verwenden. 

In diesem Azure Friday-Video mit Scott Hanselman und Kirill Gavrylyuk (DocumentDB Principal Engineering Manager) erhalten Sie weitere Informationen.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/DocumentDB-Database-as-a-Service-for-MongoDB-Developers/player]
> 


## <a name="how-to-get-started"></a>Wie geht es los?
Erstellen Sie im [Azure-Portal](https://portal.azure.com) ein DocumentDB: API für MongoDB-Konto, und tauschen Sie die Verbindung mit Ihrem neuen Konto. 

*Das ist auch schon alles!*

Detailliertere Anweisungen finden Sie unter [Erstellen eines Kontos](documentdb-create-mongodb-account.md) und [Herstellen einer Verbindung mit Ihrem Konto](documentdb-connect-mongodb-account.md).

## <a name="next-steps"></a>Nächste Schritte
* Im Tutorial [Verbinden mit einem MongoDB-Konto](documentdb-connect-mongodb-account.md) erfahren Sie, wie Sie die Information zur Verbindungszeichenfolge für Ihr Konto abrufen.
* Im Tutorial [Verwenden von MongoChef mit DocumentDB](documentdb-mongodb-mongochef.md) erfahren Sie, wie Sie eine Verbindung zwischen Ihrer DocumentDB-Datenbank und einer MongoDB-App in MongoChef erstellen.
* Weitere Informationen über DocumentDB: API für MongoDB finden Sie unter [Beispiele](documentdb-mongodb-samples.md).


