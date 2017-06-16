---
title: "Serverkonzepte in Azure-Datenbank für PostgreSQL | Microsoft-Dokumentation"
description: "Dieses Thema enthält Aspekte und Richtlinien für die Arbeit mit Azure-Datenbank für PostgreSQL-Servern."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.topic: article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: e14712b8fd68d6364f44c116448a9a8f33622a91
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---
# <a name="azure-database-for-postgresql-servers"></a>Azure-Datenbank für PostgreSQL-Server

Dieses Thema enthält Aspekte und Richtlinien für die Arbeit mit Azure-Datenbank für PostgreSQL-Servern.

## <a name="what-is-an-azure-database-for-postgresql-server"></a>Was ist ein Azure-Datenbank für PostgreSQL-Server?

Ein Azure-Datenbank für PostgreSQL-Server fungiert als zentraler Verwaltungspunkt für mehrere Datenbanken. Sie weist dasselbe PostgreSQL-Serverkonstrukt auf, das Sie möglicherweise aus lokalen Umgebungen kennen. Im Besonderen ist der PostgreSQL-Dienst verwaltet, bietet Leistungsgarantien und stellt Zugriff sowie Funktionen auf Serverebene bereit.

Ein Azure-Datenbank für PostgreSQL-Server weist folgende Eigenschaften auf:

- Sie wird im Rahmen eines Azure-Abonnements erstellt.
- Sie stellt die übergeordnete Ressource für Datenbanken dar.
- Sie stellt einen Namespace für Datenbanken bereit.
- Sie ist ein Container mit einer Semantik von hoher Lebensdauer. Beim Löschen eines Servers werden die enthaltenen Datenbanken gelöscht.
- Sie stellt Ressourcen in einer Region zusammen.
- Sie stellt einen Verbindungsendpunkt für den Server- und Datenbankzugriff (.postgresql.database.azure.com) bereit.
- Sie stellt den Bereich für Verwaltungsrichtlinien bereit, die auf die jeweiligen Datenbanken angewendet werden (Anmeldungen, Firewalls, Benutzer, Rollen, Konfigurationen etc.).
- Sie ist in mehreren Versionen verfügbar. Weitere Informationen finden Sie unter [Unterstützte PostgreSQL-Datenbankversionen](concepts-supported-versions.md).
- Sie kann von Benutzern erweitert werden. Weitere Informationen finden Sie im Artikel zu [PostgreSQL-Erweiterungen](concepts-extensions.md).

## <a name="how-do-i-connect-and-authenticate-to-an-azure-database-for-postgresql-server"></a>Wie stelle ich eine Verbindung zu einem Azure-Datenbank für PostgreSQL-Server her und authentifiziere diesen?

Mithilfe der folgenden Elemente kann ein sicherer Zugriff auf Ihre Datenbank sichergestellt werden.

|||
| :-- | :-- |
| **Authentifizierung und Autorisierung** | Der Azure-Datenbank für PostgreSQL-Server unterstützt die systemeigene PostgreSQL-Authentifizierung. Mithilfe der Anmeldeinformationen für Serveradministrator können Sie eine Verbindung zum Server herstellen und diesen authentifizieren.<br />Weitere Informationen finden Sie unter [Verwalten von Benutzern und Rollen in Azure-Datenbank für PostgreSQL](/azure/sql-database/sql-database-manage-logins). |
| **Protokoll** | Der Dienst unterstützt ein nachrichtenbasiertes Protokoll, das von PostgreSQL verwendet wird. |
| **TCP/IP** | Das Protokoll wird über TCP/IP- und Unix-Domänensockets unterstützt. |
| **Firewall** | Zum Schutz Ihrer Daten verhindert eine Firewallregel jeglichen Zugriff auf Ihren Datenbankserver oder dessen Datenbanken, bis Sie angeben, welche Computer zugriffsberechtigt sind. Weitere Informationen finden Sie unter [Firewallregeln für Azure-Datenbank für PostgreSQL-Server](concepts-firewall-rules.md). |
|||

## <a name="how-do-i-manage-a-server"></a>Wie verwalte ich einen Server?

Sie können Server für Azure-Datenbank für PostgreSQL mithilfe des Azure-Portals oder der Azure CLI verwalten.

## <a name="next-steps"></a>Nächste Schritte

- Einen Überblick über den Dienst finden Sie unter [Azure-Datenbank für PostgreSQL – Überblick](overview.md).
- Informationen zu bestimmten Ressourcenkontingenten und -beschränkungen basierend auf Ihrem **Diensttarif** finden Sie unter [Diensttarife](concepts-service-tiers.md).
- Informationen zum Herstellen einer Verbindung mit dem Dienst finden Sie unter [Datenverbindungsbibliotheken für Azure-Datenbank für PostgreSQL](concepts-connection-libraries.md).

