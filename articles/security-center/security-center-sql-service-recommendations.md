---
title: "Schützen Ihres Azure SQL-Diensts und Ihrer Daten in Azure Security Center | Microsoft Docs"
description: Dieses Dokument behandelt Empfehlungen in Azure Security Center, die zum Schutz Ihrer Daten und des Azure SQL-Diensts sowie zur Einhaltung von Sicherheitsrichtlinien beitragen.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/03/2017
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 0c3a11e9a86767641533b16de1b96b4c59bfdf51
ms.lasthandoff: 04/03/2017


---
# <a name="protecting-azure-sql-service-and-data-in-azure-security-center"></a>Schützen des Azure SQL-Diensts und Ihrer Daten in Azure Security Center
Azure Security Center analysiert den Sicherheitsstatus Ihrer Azure-Ressourcen. Werden potenzielle Sicherheitslücken erkannt, erstellt Security Center Empfehlungen, die Sie beim Konfigurieren der erforderlichen Steuerelemente unterstützen.  Die Empfehlungen gelten für folgende Azure-Ressourcentypen: virtuelle Computer (Virtual Machines, VMs), Netzwerk, SQL, Daten und Anwendungen.

In diesem Artikel werden Empfehlungen für den Azure SQL-Dienst und die Daten behandelt. Bei den Empfehlungen steht das Aktivieren der Überwachung für Azure SQL-Server und -Datenbanken sowie das Aktivieren der Verschlüsselung für SQL-Datenbanken und Ihr Azure Storage-Konto im Mittelpunkt.  Der folgenden Tabelle können Sie entnehmen, welche Empfehlungen für den SQL-Dienst und die Daten verfügbar sind und welche Aktionen jeweils ausgeführt werden, wenn Sie sie anwenden.

## <a name="available-sql-service-and-data-recommendations"></a>Verfügbare Empfehlungen für den SQL-Dienst und die Daten
| Empfehlung | Beschreibung |
| --- | --- |
| [Aktivieren der Überwachung und Bedrohungserkennung auf SQL-Servern](security-center-enable-auditing-on-sql-servers.md) |Empfiehlt Ihnen, die Überwachung und die Bedrohungserkennung für Azure SQL-Server zu aktivieren (nur Azure SQL-Dienst, keine Ausführung von SQL auf Ihren virtuellen Computern). |
| [Aktivieren der Überwachung und Bedrohungserkennung in SQL-Datenbanken](security-center-enable-auditing-on-sql-databases.md) |Empfiehlt Ihnen, die Überwachung und die Bedrohungserkennung für Azure SQL-Datenbanken zu aktivieren (nur Azure SQL-Dienst, keine Ausführung von SQL auf Ihren virtuellen Computern). |
| [Transparent Data Encryption für SQL-Datenbanken aktivieren](security-center-enable-transparent-data-encryption.md) |Empfiehlt Ihnen, die Verschlüsselung für SQL-Datenbanken zu aktivieren (nur Azure SQL-Dienst). |

## <a name="see-also"></a>Siehe auch
Weitere Informationen zu Empfehlungen für andere Arten von Azure-Ressourcen finden Sie in den folgenden Themen:

* [Schützen Ihrer virtuellen Computer in Azure Security Center](security-center-virtual-machine-recommendations.md)
* [Schützen Ihrer Anwendungen in Azure Security Center](security-center-application-recommendations.md)
* [Schützen Ihres Netzwerks in Azure Security Center](security-center-network-recommendations.md)

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md) : Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.

