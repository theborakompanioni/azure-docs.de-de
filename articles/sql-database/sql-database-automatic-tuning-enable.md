---
title: "Aktivieren der automatischen Optimierung für Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Sie können ganz einfach die automatische Optimierung für Ihre Azure SQL-Datenbank aktivieren."
services: sql-database
documentationcenter: 
author: vvasic
manager: drasumic
editor: vvasic
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: NA
ms.date: 06/05/2016
ms.author: vvasic
ms.translationtype: Human Translation
ms.sourcegitcommit: 5bbeb9d4516c2b1be4f5e076a7f63c35e4176b36
ms.openlocfilehash: b391b1f7aa37c5a06fc320ce892534187deb4959
ms.contentlocale: de-de
ms.lasthandoff: 06/13/2017


---
# <a name="enable-automatic-tuning"></a>Aktivieren der automatischen Optimierung

Azure SQL-Datenbank ist ein automatisch verwalteter Datendienst, der kontinuierlich Ihre Abfragen überwacht und Ihnen mitteilt, wie Sie die Leistung Ihrer Workload verbessern können. Sie können Empfehlungen prüfen und manuell anwenden oder Azure SQL-Datenbank die Maßnahmen automatisch anwenden lassen. Letzteres wird als **automatischer Optimierungsmodus** bezeichnet. Die automatische Optimierung kann auf der Server- oder auf der Datenbankebene aktiviert werden.

## <a name="enable-automatic-tuning-on-server"></a>Aktivieren der automatischen Optimierung für den Server

Wenn Sie die automatische Optimierung für den Azure SQL-Datenbankserver aktivieren möchten, navigieren Sie im Azure-Portal zum entsprechenden Server, und wählen Sie im Menü die Option **Automatische Optimierung** aus. Wählen Sie die gewünschten Optionen für die automatische Optimierung und anschließend **Übernehmen** aus:

![Server](./media/sql-database-automatic-tuning-enable/server.png)

Die Optionen für die automatische Optimierung des Servers werden auf alle Datenbanken auf dem Server angewendet. Standardmäßig erben alle Datenbanken die Konfiguration von ihrem übergeordneten Server, dies kann jedoch außer Kraft gesetzt und einzeln für jede Datenbank angegeben werden.

## <a name="configure-automatic-tuning-on-database"></a>Konfigurieren der automatischen Optimierung für die Datenbank

Über das Azure-Portal können Sie die automatische Optimierung für einzelne Datenbanken konfigurieren.

> [!NOTE]
> Es empfiehlt sich im Allgemeinen, die automatische Optimierung auf der Serverebene zu verwalten, damit für jede Datenbank automatisch die gleichen Konfigurationseinstellungen angewendet werden können. Konfigurieren Sie die automatische Optimierung für eine einzelne Datenbank, wenn diese sich von anderen Datenbanken auf dem gleichen Server unterscheidet.
>

Wenn Sie die automatische Optimierung für eine einzelne Datenbank aktivieren möchten, navigieren Sie im Azure-Portal zur entsprechenden Datenbank, und wählen Sie **Automatische Optimierung** aus. Durch Aktivieren des Kontrollkästchens können Sie eine einzelne Datenbank so konfigurieren, dass sie die Einstellungen der Datenbank übernimmt. Sie können aber auch eine individuelle Konfiguration für eine Datenbank angeben.

![Datenbank](./media/sql-database-automatic-tuning-enable/database.png)

Klicken Sie nach der Wahl der passenden Konfiguration auf **Übernehmen**.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zur automatischen Optimierung sowie zu ihrer Rolle bei der Verbesserung der Leistung finden Sie in [diesem Artikel](sql-database-automatic-tuning.md).
* Eine Übersicht über die Leistungsempfehlungen von Azure SQL-Datenbank finden Sie [hier](sql-database-advisor.md).
* Unter [Query Performance Insight](sql-database-query-performance.md) erfahren Sie, wie Sie die Auswirkungen Ihrer wichtigsten Abfragen auf die Leistung untersuchen können.

