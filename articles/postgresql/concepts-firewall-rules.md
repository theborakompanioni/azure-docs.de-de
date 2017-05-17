---
title: "Firewallregeln für Azure-Datenbank für PostgreSQL-Server | Microsoft-Dokumentation"
description: "Beschreibung der Firewallregeln für Azure-Datenbank für PostgreSQL-Server"
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: postgresql-database
ms.tgt_pltfrm: portal
ms.topic: article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 464a8725847271bddb19dcf71add3f5184e4771f
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---
# <a name="azure-database-for-postgresql-server-firewall-rules"></a>Firewallregeln für Azure-Datenbank für PostgreSQL-Server
Firewalls verhindern jeglichen Zugriff auf Ihren Datenbankserver, bis Sie angeben, welche Computer zugriffsberechtigt sind. Die Firewall gewährt den Serverzugriff auf der Grundlage der Ursprungs-IP-Adresse der jeweiligen Anforderung.
Zum Konfigurieren der Firewall erstellen Sie Firewallregeln, die Bereiche zulässiger IP-Adressen festlegen. Sie können Firewallregeln auf Serverebene erstellen.

**Firewallregeln:** Diese Regeln ermöglichen es Clients, auf den gesamten Azure-Datenbank für PostgreSQL-Server zuzugreifen (also auf alle Datenbanken innerhalb desselben logischen Servers). Firewallregeln auf Serverebene können über das Azure-Portal oder mithilfe von Azure CLI-Befehlen konfiguriert werden. Zum Erstellen von Firewallregeln auf Serverebene müssen Sie der Besitzer oder ein Mitwirkender des Abonnements sein.

## <a name="firewall-overview"></a>Firewallübersicht
Jeglicher Datenbankzugriff auf Ihren Azure-Datenbank für PostgreSQL-Server wird standardmäßig von der Firewall blockiert. Damit Sie Ihren Server über einen anderen Computer verwenden können, müssen Sie eine oder mehrere Firewallregeln auf Serverebene angeben, die Zugriff auf Ihren Server ermöglichen. Legen Sie mithilfe der Firewallregeln fest, welche IP-Adressbereiche aus dem Internet zugelassen werden sollen. Der Zugriff auf die Website des Azure-Portals selbst wird durch die Firewallregeln nicht beeinträchtigt.
Verbindungsversuche über das Internet und über Azure müssen zunächst die Firewall passieren, bevor sie Ihre PostgreSQL-Datenbank erreichen (wie im folgenden Diagramm dargestellt):

![Beispielfluss zur Funktionsweise der Firewall](media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a>Herstellen einer Verbindung über das Internet
Firewallregeln auf Serverebene wirken sich auf alle Datenbanken auf dem Azure-Datenbank für PostgreSQL-Server aus. Liegt die IP-Adresse der Anforderung innerhalb eines der in den Firewallregeln auf Serverebene angegebenen Bereiche, wird die Verbindung gewährt.
Liegt die IP-Adresse der Anforderung nicht innerhalb der in den Firewallregeln auf Datenbankebene oder Serverebene angegebenen Bereiche, schlägt die Verbindungsanforderung fehl.
Wenn Ihre Anwendung beispielsweise eine Verbindung mit dem JDBC-Treiber für PostgreSQL herstellt, kann bei dem Versuch eines Verbindungsaufbaus möglicherweise folgender Fehler auftreten, wenn die Verbindung durch die Firewall blockiert wird.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: FATAL: Kein pg\_hba.conf-Eintrag für Host „123.45.67.890“, Benutzer „adminuser“, Datenbank „postgresql“, SSL

## <a name="programmatically-managing-firewall-rules"></a>Programmgesteuertes Verwalten von Firewallregeln
Zusätzlich zum Azure-Portal können Firewallregeln programmgesteuert mithilfe der Azure CLI verwaltet werden.
Weitere Informationen finden Sie unter [Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für PostgreSQL mithilfe der Azure CLI](howto-manage-firewall-using-cli.md).

## <a name="troubleshooting-the-database-firewall"></a>Problembehandlung der Datenbankfirewall
Wenn der Zugriff auf den Microsoft Azure-Datenbank für PostgreSQL-Serverdienst nicht das erwartete Verhalten aufweist, sind folgende Punkte zu beachten:

* **Änderungen an der Zulassungsliste sind noch nicht wirksam:** Änderungen an der Firewallkonfiguration von Azure-Datenbank für PostgreSQL-Servern werden möglicherweise erst nach fünf Minuten wirksam.

* **Die Anmeldung ist nicht autorisiert, oder ein falsches Kennwort wurde verwendet:** Wenn eine Anmeldung nicht über Berechtigungen auf dem Azure-Datenbank für PostgreSQL-Server verfügt, oder das verwendete Kennwort falsch ist, wird die Verbindung zum Azure-Datenbank für PostgreSQL-Server verweigert. Durch das Erstellen einer Firewalleinstellung wird Clients lediglich die Möglichkeit gegeben, eine Verbindung mit dem Server herzustellen. Jeder Client muss die erforderlichen Sicherheitsanmeldeinformationen bereitstellen.

Beispielsweise kann bei der Verwendung eines JDBC-Clients folgende Fehlermeldung angezeigt werden.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: FATAL: Fehler bei der Kennwortauthentifizierung für Benutzer „IhrBenutzername“

* **Dynamische IP-Adresse:** Wenn Sie über eine Internetverbindung mit dynamischer IP-Adressierung verfügen und Probleme beim Passieren der Firewall auftreten, können Sie eine der folgenden Lösungen ausprobieren:

* Fragen Sie Ihren Internetdienstanbieter (ISP) nach dem IP-Adressbereich, der den Clientcomputern zugewiesen ist, mit denen auf den Azure-Datenbank für PostgreSQL-Server zugegriffen wird, und fügen Sie dann den IP-Adressbereich als Firewallregel hinzu.

* Verwenden Sie stattdessen die statische IP-Adressierung für die Clientcomputer, und fügen Sie dann die IP-Adressen als Firewallregeln hinzu.

## <a name="next-steps"></a>Nächste Schritte
Artikel zum Erstellen von Firewallregeln auf Serverebene und auf Datenbankebene finden Sie hier:
* [Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für PostgreSQL mithilfe des Azure-Portals](howto-manage-firewall-using-portal.md)
* [Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für PostgreSQL mithilfe der Azure CLI](howto-manage-firewall-using-cli.md)
