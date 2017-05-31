---
title: "Verschlüsselung ruhender Daten von Datenbanken – Azure Cosmos DB | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Azure Cosmos DB eine Standardverschlüsselung für alle Daten bereitstellt."
services: cosmosdb
author: voellm
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 99725c52-d7ca-4bfa-888b-19b1569754d3
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/31/2017
ms.author: voellm
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 8248b429cf9b7fdfc709a68c4d1e88b7b660020b
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---

# <a name="azure-cosmos-db-database-encryption-at-rest"></a>Verschlüsselung ruhender Daten von Datenbanken mit Azure Cosmos DB

Verschlüsselung ruhender Daten ist ein Ausdruck, der sich im Allgemeinen auf die Verschlüsselung von Daten auf permanenten Speichergeräten wie Solid State-Laufwerke (SSDs) und-Festplattenlaufwerke (HDDs) bezieht.  Azure Cosmos DB speichert die primären Datenbanken auf SSD und seine Medienanhänge und Sicherungen in Azure-Blobs, die in der Regel auf Festplatte (HDD) gesichert werden.  Mit der Veröffentlichung der Verschlüsselung von ruhenden Daten für Azure Cosmos DB werden Ihre gesamten Datenbanken, Medienanhänge und Sicherungen jetzt verschlüsselt.  Dies bedeutet, dass Ihre Daten jetzt während der Übertragung (über das Netzwerk) verschlüsselt und im Ruhezustand (permanenter Speicher) sind, wodurch Sie eine End-to-End-Verschlüsselung erhalten.

Als PaaS-Dienst haben wir hart daran gearbeitet, Azure Cosmos DB sehr benutzerfreundlich zu gestalten.  In diesem Sinn werden alle in Azure Cosmos DB gespeicherten Benutzerdaten im Ruhezustand und bei der Übertragung verschlüsselt, ohne dass vom Benutzer Maßnahmen ergriffen werden müssen.  Eine andere Möglichkeit, um dies auszudrücken: Die Verschlüsselung ruhender Daten ist standardmäßig „aktiviert“.  Es gibt keine Steuerelemente, um sie ein- bzw. auszuschalten. Wir haben dieses Feature bereitgestellt, während wir weiterhin unsere [SLAs zur Verfügbarkeit und Leistung](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/) einhalten.

## <a name="how-does-encryption-at-rest-work"></a>Wie funktioniert die Verschlüsselung ruhender Daten?

Die Verschlüsselung ruhender Daten wird über eine Reihe von Sicherheitstechnologien implementiert, einschließlich der Speichersysteme für sichere Schlüssel, verschlüsselter Netzwerke und Kryptografie-APIs.  Das folgende Diagramm veranschaulicht die Trennung zwischen der Speicherung von verschlüsselten Daten und der Verwaltung von Schlüsseln.  Systeme, die Daten verarbeiten und entschlüsseln, müssen mit Systemen kommunizieren, die Schlüssel verwalten.

![Entwurfsdiagramm](./media/documentdb-nosql-database-encryption-at-rest/design-diagram.png)

Der grundlegende Ablauf bei einer Benutzeranforderung sieht wie folgt aus:
- Das Benutzerdatenbankkonto wird bereitgestellt und die Speicherschlüssel werden über eine Anforderung an den Ressourcenanbieter für den Verwaltungsdienst abgerufen.
- Ein Benutzer erstellt über HTTPS bzw. über den sicheren Transport (die Details werden in den SDKs zusammengefasst) eine Verbindung mit Azure Cosmos DB.
- Der Benutzer sendet ein zu speicherndes JSON-Dokument über die zuvor erstellte sichere Verbindung.
- Das JSON-Dokument wird indiziert, sofern der Benutzer die Indizierung nicht deaktiviert hat.
- Sowohl das JSON-Dokument als auch die Indexdaten werden in den sicheren Speicher geschrieben.
- In regelmäßigen Abständen werden Daten aus dem sicheren Speicher gelesen und in verschlüsseltem Azure Blob Storage gesichert.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="q-how-much-more-does-azure-storage-cost-if-sse-is-enabled"></a>F: Wie hoch sind die Mehrkosten für Azure Storage, wenn SSE aktiviert ist?
A: Es fallen keine zusätzlichen Kosten an.

### <a name="q-who-manages-the-encryption-keys"></a>F: Wer verwaltet die Verschlüsselungsschlüssel?
A: Die Schlüssel werden von Microsoft verwaltet.

### <a name="q-how-often-are-encryption-keys-rotated"></a>F: Wie oft werden die Verschlüsselungsschlüssel ausgetauscht?
A: Microsoft hat eine Reihe von internen Richtlinien festgelegt, die von DocumentDB eingehalten werden.  Obwohl die bestimmten Richtlinien nicht veröffentlicht werden, veröffentlicht Microsoft den (Security Development Lifecycle, SDL)[https://www.microsoft.com/sdl/default.aspx], der als Teilmenge der internen Anleitung betrachtet wird und über hilfreiche bewährte Methoden für Entwickler verfügt.

### <a name="q-can-i-use-my-own-encryption-keys"></a>F: Kann ich meine eigenen Verschlüsselungsschlüssel verwenden?
A: Azure Cosmos DB ist ein PaaS-Dienst und wir haben hart daran gearbeitet, den Dienst benutzerfreundlich zu gestalten.  Wir haben festgestellt, dass diese Frage häufig zu Proxys zur Einhaltung einer Compliance wie PCI-DSS gestellt wird.  Im Rahmen der Erstellung dieses Features haben wir mit Complianceprüfern zusammengearbeitet, um sicherzustellen, dass Kunden, die Azure Cosmos DB verwenden, ihre Anforderungen erfüllen können, ohne selbst Schlüssel verwalten zu müssen.
Aus diesem Grund bieten wir Benutzern derzeit die Option, sich die Schlüsselverwaltung selbst aufzubürden.


### <a name="q-what-regions-have-encryption-turned-on"></a>F: Für welche Regionen ist die Verschlüsselung aktiviert?
A: Die Verschlüsselung ist für alle Azure Cosmos DB-Regionen und für alle Benutzerdaten aktiviert.

### <a name="q-does-encryption-affect-the-performance-latency-and-throughput-slas"></a>F: Wirkt sich die Verschlüsselung auf die SLAs hinsichtlich Leistung, Wartezeit und Durchsatz aus?
A: Es gibt keine Auswirkungen oder Änderungen hinsichtlich der Leistungs-SLAs, nachdem die Verschlüsselung ruhender Daten für alle vorhandenen und neuen Konten aktiviert ist.  Informationen zu den aktuellsten Garantien finden Sie auf der Seite für [Leistungs-SLAs](https://azure.microsoft.com/support/legal/sla/documentdb).

### <a name="q-does-the-local-emulator-support-encryption-at-rest"></a>F: Unterstützt der lokale Emulator die Verschlüsselung ruhender Daten?
A: Der Emulator ist ein eigenständiges Entwicklungs-/Testtool und verwendet keine Schlüsselverwaltungsdienste, die vom verwalteten DocumentDB-Dienst verwendet werden. Unsere Empfehlung ist, BitLocker auf Laufwerken zu aktivieren, auf denen Sie vertrauliche Eumlatortestdaten speichern. Der (Emulator unterstützt das Wechseln des Standarddatenverzeichnisses)[https://docs.microsoft.com/azure/documentdb/documentdb-nosql-local-emulator] sowie die Verwendung eines bekannten Speicherorts.

## <a name="next-steps"></a>Nächste Schritte

Einen Überblick über die Azure Cosmos DB-Sicherheit und die neuesten Verbesserungen finden Sie unter [Azure Cosmos DB-Sicherheit für Datenbanken](documentdb-nosql-database-security.md).

Weitere Informationen zu Microsoft-Zertifizierungen finden Sie im [Azure Trust Center](https://azure.microsoft.com/en-us/support/trust-center/).

