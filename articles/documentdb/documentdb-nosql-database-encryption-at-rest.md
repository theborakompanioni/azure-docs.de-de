---
title: "Verschlüsselung ruhender Daten von NoSQL-Datenbanken – Azure DocumentDB | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Azure DocumentDB eine Standardverschlüsselung für alle NoSQL-Daten bereitstellt."
services: documentdb
author: voellm
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 99725c52-d7ca-4bfa-888b-19b1569754d3
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/31/2017
ms.author: voellm
ms.translationtype: Human Translation
ms.sourcegitcommit: 8f291186c6a68dea8aa00b846a2e6f3ad0d7996c
ms.openlocfilehash: 9c97f76d581935cb85f33b0016c192ded3b6f880
ms.contentlocale: de-de
ms.lasthandoff: 04/28/2017


---

# <a name="documentdb-nosql-database-encryption-at-rest"></a>Verschlüsselung ruhender Daten von NoSQL-Datenbanken mit DocumentDB

Verschlüsselung ruhender Daten ist ein Ausdruck, der sich im Allgemeinen auf die Verschlüsselung von Daten auf permanenten Speichergeräten wie Solid State-Laufwerke (SSDs) und-Festplattenlaufwerke (HDDs) bezieht.  DocumentDB speichert die primären Datenbanken auf SSD und seine Medienanhänge und Sicherungen in Azure-Blobs, die in der Regel auf Festplatte (HDD) gesichert werden.  Mit der Veröffentlichung der Verschlüsselung von ruhenden Daten für DocumentDB werden Ihre gesamten Datenbanken, Medienanhänge und Sicherungen jetzt verschlüsselt.  Dies bedeutet, dass Ihre Daten jetzt während der Übertragung (über das Netzwerk) verschlüsselt und im Ruhezustand (permanenter Speicher) sind, wodurch Sie eine End-to-End-Verschlüsselung erhalten.

Als PaaS-Dienst haben wir hart daran gearbeitet, DocumentDB sehr benutzerfreundlich zu gestalten.  In diesem Sinn werden alle in DocumentDB gespeicherten Benutzerdaten im Ruhezustand und bei der Übertragung verschlüsselt, ohne dass vom Benutzer Maßnahmen ergriffen werden müssen.  Eine andere Möglichkeit, um dies auszudrücken: Die Verschlüsselung ruhender Daten ist standardmäßig „aktiviert“.  Es gibt keine Steuerelemente, um sie ein- bzw. auszuschalten. Wir haben dieses Feature bereitgestellt, während wir weiterhin unsere [SLAs zur Verfügbarkeit und Leistung](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/) einhalten.

## <a name="how-does-encryption-at-rest-work"></a>Wie funktioniert die Verschlüsselung ruhender Daten?

Die Verschlüsselung ruhender Daten wird über eine Reihe von Sicherheitstechnologien implementiert, einschließlich der Speichersysteme für sichere Schlüssel, verschlüsselter Netzwerke und Kryptografie-APIs.  Das folgende Diagramm veranschaulicht die Trennung zwischen der Speicherung von verschlüsselten Daten und der Verwaltung von Schlüsseln.  Systeme, die Daten verarbeiten und entschlüsseln, müssen mit Systemen kommunizieren, die Schlüssel verwalten.

![Entwurfsdiagramm](./media/documentdb-nosql-database-encryption-at-rest/design-diagram.png)

Der grundlegende Ablauf bei einer Benutzeranforderung sieht wie folgt aus:
- Das Benutzerdatenbankkonto wird bereitgestellt und die Speicherschlüssel werden über eine Anforderung an den Ressourcenanbieter für den Verwaltungsdienst abgerufen.
- Ein Benutzer erstellt über HTTPS bzw. über den sicheren Transport (die Details werden in den SDKs zusammengefasst) eine Verbindung mit DocumentDB.
- Der Benutzer sendet ein zu speicherndes JSON-Dokument über die zuvor erstellte sichere Verbindung.
- Das JSON-Dokument wird indiziert, sofern der Benutzer die Indizierung nicht deaktiviert hat.
- Sowohl das JSON-Dokument als auch die Indexdaten werden in den sicheren Speicher geschrieben.
- In regelmäßigen Abständen werden Daten aus dem sicheren Speicher gelesen und in verschlüsseltem Azure Blob Storage gesichert.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="q-how-much-more-does-azure-storage-cost-if-sse-is-enabled"></a>F: Wie hoch sind die Mehrkosten für Azure Storage, wenn SSE aktiviert ist?
A: Es fallen keine zusätzlichen Kosten an.

### <a name="q-who-manages-the-encryption-keys"></a>F: Wer verwaltet die Verschlüsselungsschlüssel?
A: Die Schlüssel werden von Microsoft verwaltet.

### <a name="q-can-i-use-my-own-encryption-keys"></a>F: Kann ich meine eigenen Verschlüsselungsschlüssel verwenden?
A: DocumentDB ist ein PaaS-Dienst und wir haben hart daran gearbeitet, den Dienst benutzerfreundlich zu gestalten.  Wir haben festgestellt, dass diese Frage häufig zu Proxys zur Einhaltung einer Compliance wie PCI-DSS gestellt wird.  Im Rahmen der Erstellung dieses Features haben wir mit Complianceprüfern zusammengearbeitet, um sicherzustellen, dass Kunden, die DocumentDB verwenden, ihre Anforderungen erfüllen können, ohne selbst Schlüssel verwalten zu müssen.
Aus diesem Grund bieten wir Benutzern derzeit die Option, sich die Schlüsselverwaltung selbst aufzubürden.

### <a name="q-what-regions-have-encryption-turned-on"></a>F: Für welche Regionen ist die Verschlüsselung aktiviert?
A: Die Verschlüsselung ist für alle DocumentDB-Regionen und für alle Benutzerdaten aktiviert.

## <a name="next-steps"></a>Nächste Schritte

Einen Überblick über die DocumentDB-Sicherheit und die neuesten Verbesserungen finden Sie unter [DocumentDB-Sicherheit für NoSQL-Datenbanken](documentdb-nosql-database-security.md).

Weitere Informationen zu Microsoft-Zertifizierungen finden Sie im [Azure Trust Center](https://azure.microsoft.com/en-us/support/trust-center/).

