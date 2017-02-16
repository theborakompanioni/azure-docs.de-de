---
title: "Was ist DocumentDB-Protokollunterstützung für MongoDB? | Microsoft Docs"
description: "Was ist DocumentDB-Protokollunterstützung für MongoDB? Sie ermöglicht die Verwendung von Azure DocumentDB, einem verwalteten cloudbasierten Dienst, als Datenspeicher für Apps, die für MongoDB geschrieben wurden."
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
ms.date: 11/23/2016
ms.author: anhoh
translationtype: Human Translation
ms.sourcegitcommit: 0703fa4f4d3eb9f23aa9b3c0b90267fac9557f13
ms.openlocfilehash: 876fff6a63ccf1a3c10a6852927c60b454436954


---
# <a name="what-is-documentdb-protocol-support-for-mongodb"></a>Was ist DocumentDB-Protokollunterstützung für MongoDB?

DocumentDB-Datenbanken können jetzt als Datenspeicher für Apps verwendet werden, die für MongoDB geschrieben wurden. Dies bedeutet, dass Ihre für MongoDB geschriebene Anwendung durch Nutzung vorhandener [Treiber](https://docs.mongodb.org/ecosystem/drivers/) für MongoDB-Datenbanken jetzt mit DocumentDB kommunizieren und DocumentDB-Datenbanken statt MongoDB-Datenbanken verwenden kann. In vielen Fällen können Sie von der Verwendung von MongoDB zu DocumentDB wechseln, indem Sie einfach eine Verbindungszeichenfolge ändern. Mit dieser Funktion können Kunden – unter Verwendung der vollständig verwalteten und skalierbaren NoSQL-Datenbanken von DocumentDB – ganz einfach MongoDB-Datenbankanwendungen in der Azure-Cloud erstellen und ausführen und weiterhin die vorhandenen Kenntnisse und Tools für MongoDB verwenden.

## <a name="what-is-the-benefit-of-using-documentdb-protocol-support-for-mongodb"></a>Welche Vorteile bietet die Verwendung der DocumentDB-Protokollunterstützung für MongoDB?
**Keine Serververwaltung**: DocumentDB ist ein vollständig verwalteter Dienst, d.h. Sie müssen selbst keine Infrastruktur oder virtuellen Computer verwalten. DocumentDB ist in mehr als 20 [Azure-Regionen](https://azure.microsoft.com/regions/services/) verfügbar.

**Unbegrenzte Skalierung**: Sie können Durchsatz und Speicherressourcen unabhängig und elastisch skalieren. Sie können weitere Kapazität hinzufügen, um problemlos Millionen von Anforderungen pro Sekunde zu verarbeiten.

**Unternehmensniveau**: DocumentDB unterstützt mehrere lokale Replikate, um eine Verfügbarkeit von 99,99 % sowie Datenschutz bei lokalen und regionalen Ausfällen bereitzustellen. DocumentDB verfügt über [Kompatibilitätszertifizierungen](https://www.microsoft.com/trustcenter) und Sicherheitsfunktionen auf Unternehmensniveau. 

**Kompatibilität mit MongoDB**: Die DocumentDB-Protokollunterstützung für MongoDB wurde für Kompatibilität mit MongoDB konzipiert. Für die Arbeit mit DocumentDB können Sie Ihre vorhandenen Codezeilen, Anwendungen, Treiber und Tools verwenden. 

## <a name="how-to-get-started"></a>Wie geht es los?
Erstellen Sie im [Azure-Portal](https://portal.azure.com) ein DocumentDB-Konto mit Protokollunterstützung für MongoDB, und verweisen Sie die Verbindung auf Ihr neues Konto. 

*Das ist auch schon alles!*

Detailliertere Anweisungen finden Sie unter [Erstellen eines Kontos](documentdb-create-mongodb-account.md) und [Herstellen einer Verbindung mit Ihrem Konto](documentdb-connect-mongodb-account.md).

## <a name="next-steps"></a>Nächste Schritte
* Befolgen Sie die Anweisungen im Tutorial [Erstellen eines DocumentDB-Kontos mit Protokollunterstützung für MongoDB](documentdb-create-mongodb-account.md), um ein DocumentDB-Konto zu erstellen.
* Im Tutorial [Herstellen einer Verbindung mit einem DocumentDB-Konto mit Protokollunterstützung für MongoDB](documentdb-connect-mongodb-account.md) erfahren Sie, wie Sie die Verbindungszeichenfolge für Ihr Konto abrufen.
* Im Tutorial [Verwenden von MongoChef mit einem DocumentDB-Konto mit Protokollunterstützung für MongoDB](documentdb-mongodb-mongochef.md) erfahren Sie, wie Sie eine Verbindung zwischen Ihrer DocumentDB-Datenbank und einer MongoDB-App in MongoChef erstellen.
* Untersuchen Sie DocumentDB mit Protokollunterstützung für MongoDB anhand von [Beispielen](documentdb-mongodb-samples.md).




<!--HONumber=Nov16_HO4-->


