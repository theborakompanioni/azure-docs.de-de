---
title: "Schützen Ihres Azure SQL-Diensts in Azure Security Center | Microsoft Docs"
description: Dieses Dokument behandelt Empfehlungen in Azure Security Center, die zum Schutz des Azure SQL-Diensts sowie zur Einhaltung von Sicherheitsrichtlinien beitragen.
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
ms.date: 11/16/2016
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: 174546d59124296711731de6c8d8929bada56baf
ms.openlocfilehash: 346e8edda93213bde7a9f0928641cb125be01f31


---
# <a name="protecting-azure-sql-service-in-azure-security-center"></a>Schützen Ihres Azure SQL-Diensts in Azure Security Center
Azure Security Center analysiert den Sicherheitsstatus Ihrer Azure-Ressourcen. Werden potenzielle Sicherheitslücken erkannt, erstellt Security Center Empfehlungen, die Sie beim Konfigurieren der erforderlichen Steuerelemente unterstützen.  Die Empfehlungen gelten für folgende Azure-Ressourcentypen: virtuelle Computer (Virtual Machines, VMs), Netzwerk, SQL und Anwendungen.

In diesem Artikel werden Empfehlungen für den Azure SQL-Dienst behandelt.  Bei den Empfehlungen für den Azure SQL-Dienst steht das Aktivieren der Überwachung für Azure SQL-Server und -Datenbanken sowie das Aktivieren der Verschlüsselung für SQL-Datenbanken im Mittelpunkt.  Entnehmen Sie der folgenden Tabelle, welche Empfehlungen für den SQL-Dienst verfügbar sind und welche Aktionen jeweils ausgeführt werden, wenn Sie sie anwenden.

## <a name="available-sql-service-recommendations"></a>Verfügbare Empfehlungen für den SQL-Dienst
| Empfehlung | Beschreibung |
| --- | --- |
| [Überwachung von SQL-Server aktivieren](security-center-enable-auditing-on-sql-servers.md) |Empfiehlt Ihnen, die Überwachung für Azure SQL-Server zu aktivieren (nur Azure SQL-Dienst, keine Ausführung von SQL auf Ihren virtuellen Maschinen). |
| [Überwachung der SQL-Datenbank aktivieren](security-center-enable-auditing-on-sql-databases.md) |Empfiehlt Ihnen, die Überwachung für Azure SQL-Datenbanken zu aktivieren (nur Azure SQL-Dienst, keine Ausführung von SQL auf Ihren virtuellen Maschinen). |
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



<!--HONumber=Nov16_HO3-->


