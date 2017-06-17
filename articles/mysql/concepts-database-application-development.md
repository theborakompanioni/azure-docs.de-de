---
title: "Entwicklung von Datenbankanwendungen für Azure-Datenbank für MySQL – Überblick | Microsoft-Dokumentation"
description: "Einführung in Entwurfsüberlegungen, die Entwickler beim Schreiben des Anwendungscodes zum Herstellen einer Verbindung zu Azure-Datenbank für MySQL befolgen sollten"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 5f8bb1631ccd6dcfe1354e3b4c0897c587149b1d
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="application-development-overview-for-azure-database-for-mysql"></a>Entwicklung von Anwendungen für Azure-Datenbank für MySQL – Überblick 
In diesem Artikel werden Entwurfsüberlegungen besprochen, die Entwickler beim Schreiben des Anwendungscodes zum Herstellen einer Verbindung zu Azure-Datenbank für MySQL befolgen sollten. 

> [!TIP]
> Ein Tutorial, das die Erstellung eines Servers, Erstellung einer serverbasierten Firewall, Anzeige von Servereigenschaften, Erstellung einer Datenbank, Herstellen einer Verbindung mit Workbench und mysql.exe und Abfrage derselbigen zeigt, finden Sie unter [Entwerfen der ersten Azure-Datenbank für MySQL](tutorial-design-database-using-portal.md)

## <a name="language-and-platform"></a>Sprache und Plattform
Für eine Vielzahl von Programmiersprachen und Plattformen sind Codebeispiele verfügbar. Die Links zu den Codebeispielen finden Sie in folgendem Thema: [Konnektivitätsbibliotheken zur Verbindung mit Azure-Datenbank für MySQL](concepts-connection-libraries.md)

## <a name="tools"></a>Tools
Azure-Datenbank für MySQL verwendet die MySQL Community-Version, die mit allgemeinen MySQL-Verwaltungstools wie Workbench oder MySQL-Dienstprogrammen (z.B. mysql.exe, [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql)) kompatibel ist. Sie können auch über das Azure-Portal, die Azure CLI und REST-APIs mit dem Datenbankdienst interagieren.

## <a name="resource-limitations"></a>Ressourcenbeschränkungen
Azure-Datenbank für MySQL verwaltet die für einen Server verfügbaren Ressourcen anhand von zwei verschiedenen Mechanismen: 
- Ressourcenkontrolle 
- Durchsetzung von Grenzwerten

## <a name="security"></a>Sicherheit
Azure-Datenbank für MySQL stellt Ressourcen zum Einschränken des Zugriffs, Schützen von Daten, Konfigurieren von Benutzern und Rollen sowie Überwachen von Aktivitäten in einer MySQL-Datenbank bereit.

## <a name="authentication"></a>Authentifizierung
Azure-Datenbank für MySQL unterstützt die Serverauthentifizierung von Benutzern und Anmeldungen.

## <a name="resiliency"></a>Resilienz
Wenn beim Herstellen einer Verbindung mit der MySQL-Datenbank ein vorübergehender Fehler auftritt, sollte Ihr Code den Aufruf wiederholen. Die Wiederholungslogik sollte Backoff-Logik verwenden, damit die SQL-Datenbank nicht unnötig überlastet wird, wenn mehrere Clients die Wiederholung gleichzeitig durchführen.

- Codebeispiele: Codebeispiele, die die Wiederholungslogik veranschaulichen, finden Sie in den Beispielen für die Sprache Ihrer Wahl: [Konnektivitätsbibliotheken zum Herstellen einer Verbindung mit Azure-Datenbank für MySQL](concepts-connection-libraries.md).

## <a name="managing-connections"></a>Verwalten von Verbindungen
Datenbankverbindungen stellen eine im begrenzten Umfang verfügbare Ressource dar. Daher sollten Verbindungen beim Zugriff auf Ihre MySQL-Datenbank sinnvoll eingesetzt werden, um eine bessere Leistung zu erzielen.
- Greifen Sie anhand von Verbindungspooling oder persistenten Verbindungen auf die Datenbank zu.
- Greifen Sie über eine kurze Verbindungsdauer auf die Datenbank zu. 
- Verwenden Sie beim Verbindungsversuch die Wiederholungslogik in Ihrer Anwendung, um Fehler durch Überschreitung der maximal zulässigen Anzahl an gleichzeitigen Verbindungen abzufangen. Legen Sie in der Wiederholungslogik eine kurze Verzögerung fest, und warten Sie dann eine gewisse Zeit, bevor Sie weitere Verbindungsversuche unternehmen.
