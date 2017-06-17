---
title: "Firewallregeln für Azure-Datenbank für MySQL-Server | Microsoft-Dokumentation"
description: "Beschreibung der Firewallregeln für Azure-Datenbank für MySQL-Server"
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
ms.openlocfilehash: 511cc50c28b9b114b763f3c46cb1e26f15575349
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="azure-database-for-mysql-server-firewall-rules"></a>Firewallregeln für Azure-Datenbank für MySQL-Server
Firewalls verhindern jeglichen Zugriff auf Ihren Datenbankserver, bis Sie angeben, welche Computer zugriffsberechtigt sind. Die Firewall gewährt den Serverzugriff auf der Grundlage der Ursprungs-IP-Adresse der jeweiligen Anforderung.

Zum Konfigurieren der Firewall erstellen Sie Firewallregeln, die Bereiche zulässiger IP-Adressen festlegen. Sie können Firewallregeln auf Serverebene erstellen.

**Firewallregeln:** Diese Regeln ermöglichen es Clients, auf den gesamten Azure-Datenbank für MySQL-Server zuzugreifen (also auf alle Datenbanken innerhalb desselben logischen Servers). Firewallregeln auf Serverebene können über das Azure-Portal oder mithilfe von Azure CLI-Befehlen konfiguriert werden. Zum Erstellen von Firewallregeln auf Serverebene müssen Sie der Besitzer oder ein Mitwirkender des Abonnements sein.

## <a name="firewall-overview"></a>Firewallübersicht
Jeglicher Datenbankzugriff auf Ihren Azure-Datenbank für MySQL-Server wird standardmäßig von der Firewall blockiert. Damit Sie Ihren Server über einen anderen Computer verwenden können, müssen Sie eine oder mehrere Firewallregeln auf Serverebene angeben, die Zugriff auf Ihren Server ermöglichen. Legen Sie mithilfe der Firewallregeln fest, welche IP-Adressbereiche aus dem Internet zugelassen werden sollen. Der Zugriff auf die Website des Azure-Portals selbst wird durch die Firewallregeln nicht beeinträchtigt.

Verbindungsversuche über das Internet und Azure müssen zunächst die Firewall passieren, bevor sie Ihren Azure-Datenbank für MySQL-Server erreichen (wie im folgenden Diagramm dargestellt):

![Beispielfluss zur Funktionsweise der Firewall](./media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a>Herstellen einer Verbindung über das Internet
Firewallregeln auf Serverebene wirken sich auf alle Datenbanken auf dem Azure-Datenbank für MySQL-Server aus.

Liegt die IP-Adresse der Anforderung innerhalb eines der in den Firewallregeln auf Serverebene angegebenen Bereiche, wird die Verbindung gewährt.

Liegt die IP-Adresse der Anforderung nicht innerhalb der in den Firewallregeln auf Datenbankebene oder Serverebene angegebenen Bereiche, schlägt die Verbindungsanforderung fehl.

## <a name="programmatically-managing-firewall-rules"></a>Programmgesteuertes Verwalten von Firewallregeln
Zusätzlich zum Azure-Portal können Firewallregeln programmgesteuert mithilfe der Azure CLI verwaltet werden. Weitere Informationen finden Sie unter [Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für MySQL mithilfe der Azure CLI](./howto-manage-firewall-using-cli.md).

## <a name="troubleshooting-the-database-firewall"></a>Problembehandlung der Datenbankfirewall
Wenn der Zugriff auf den Microsoft Azure-Datenbank für MySQL-Serverdienst nicht das erwartete Verhalten aufweist, sind folgende Punkte zu beachten:

* **Änderungen an der Zulassungsliste sind noch nicht wirksam:** Änderungen an der Firewallkonfiguration für Azure-Datenbank für MySQL-Server werden möglicherweise erst nach fünf Minuten wirksam.

* **Die Anmeldung ist nicht autorisiert, oder ein falsches Kennwort wurde verwendet:** Wenn eine Anmeldung nicht über Berechtigungen auf dem Azure-Datenbank für MySQL-Server verfügt, oder das verwendete Kennwort falsch ist, wird die Verbindung zum Azure-Datenbank für MySQL-Server verweigert. Durch das Erstellen einer Firewalleinstellung wird Clients lediglich die Möglichkeit gegeben, eine Verbindung mit dem Server herzustellen. Jeder Client muss die erforderlichen Sicherheitsanmeldeinformationen bereitstellen.

* **Dynamische IP-Adresse:** Wenn Sie über eine Internetverbindung mit dynamischer IP-Adressierung verfügen und Probleme beim Passieren der Firewall auftreten, können Sie eine der folgenden Lösungen ausprobieren:

* Fragen Sie Ihren Internetdienstanbieter (ISP) nach dem IP-Adressbereich, der den Clientcomputern zugewiesen ist, mit denen auf den Azure-Datenbank für MySQL-Server zugegriffen wird, und fügen Sie dann den IP-Adressbereich als Firewallregel hinzu.

* Verwenden Sie stattdessen die statische IP-Adressierung für die Clientcomputer, und fügen Sie dann die IP-Adressen als Firewallregeln hinzu.

## <a name="next-steps"></a>Nächste Schritte

[Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für MySQL mithilfe des Azure-Portals](./howto-manage-firewall-using-portal.md)
[Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für MySQL mithilfe der Azure CLI](./howto-manage-firewall-using-cli.md)

