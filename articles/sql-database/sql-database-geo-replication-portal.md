---
title: "Konfigurieren der Georeplikation für eine Azure SQL-Datenbank mit dem Azure-Portal | Microsoft Docs"
description: "Konfigurieren der Georeplikation für die Azure SQL-Datenbank mit dem Azure-Portal"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: d0b29822-714f-4633-a5ab-fb1a09d43ced
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/22/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 8d988aa55d053d28adcf29aeca749a7b18d56ed4
ms.openlocfilehash: fe2d2ef731fb94c7e4e8da0e518bcef8c1ada650
ms.lasthandoff: 02/16/2017


---
# <a name="configure-active-geo-replication-for-azure-sql-database-with-the-azure-portal"></a>Konfigurieren der aktiven Georeplikation für Azure SQL-Datenbank mit dem Azure-Portal

In diesem Artikel erfahren Sie, wie Sie die aktive Georeplikation für eine SQL-Datenbank mit dem [Azure-Portal](http://portal.azure.com)konfigurieren.

Informationen zum Initiieren eines Failovers mit dem Azure-Portal finden Sie unter [Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit dem Azure-Portal](sql-database-geo-replication-failover-portal.md).

> [!NOTE]
> Die aktive Georeplikation (lesbare sekundäre Datenbanken) ist jetzt für alle Datenbanken in allen Diensttarifen verfügbar. Im April 2017 wird der nicht lesbare sekundäre Typ eingestellt, und vorhandene nicht lesbare Datenbanken werden automatisch zu lesbaren sekundären Datenbanken aktualisiert.
> 
> 

Wenn Sie die aktive Georeplikation über das Azure-Portal konfigurieren möchten, benötigen Sie folgende Ressource:

* Eine Azure SQL-Datenbank: Die primäre Datenbank, die in eine andere geografische Region repliziert werden soll.

> [!Note]
Die aktive Georeplikation muss zwischen Datenbanken innerhalb desselben Abonnements vorhanden sein.

## <a name="add-a-secondary-database"></a>Hinzufügen einer sekundären Datenbank
Mit den folgenden Schritten wird eine neue sekundäre Datenbank in einer Partnerschaft für die Georeplikation erstellt.  

Zum Hinzufügen einer sekundären Datenbank müssen Sie der Besitzer oder Mitbesitzer des Abonnements sein.

Die sekundäre Datenbank hat den gleichen Namen wie die primäre Datenbank und standardmäßig auch den gleichen Servicelevel. Die sekundäre Datenbank kann eine Einzeldatenbank oder eine Datenbank in einem Pool für elastische Datenbanken sein. Weitere Informationen finden Sie unter [Dienstebenen](sql-database-service-tiers.md).
Nachdem die sekundäre Datenbank erstellt und das Seeding ausgeführt wurde, beginnt die Replikation der Daten von der primären Datenbank in die neue sekundäre Datenbank.

> [!NOTE]
> Wenn die Partnerdatenbank bereits vorhanden ist (z.B. aufgrund der Beendigung einer vorherigen Georeplikationsbeziehung), tritt für den Befehl ein Fehler auf.
> 
> 

### <a name="add-secondary"></a>Sekundäre Datenbank hinzufügen
1. Navigieren Sie im [Azure-Portal](http://portal.azure.com) zu der Datenbank, die Sie für die Georeplikation einrichten möchten.
2. Wählen Sie auf der Seite „SQL-Datenbank“ die Option **Georeplikation**, und wählen Sie dann die Region aus, in der die sekundäre Datenbank erstellt werden soll. Sie können jede Region außer der Region auswählen, in der die primäre Datenbank gehostet wird. Es empfiehlt sich jedoch, die Partnerregion im [Regionspaar](../best-practices-availability-paired-regions.md) zu verwenden.
   
    ![Konfigurieren der Georeplikation](./media/sql-database-geo-replication-portal/configure-geo-replication.png)
3. Wählen Sie den Server und den Tarif für die sekundäre Datenbank aus bzw. konfigurieren Sie Server und Tarif.
   
    ![Konfigurieren der sekundären Datenbank](./media/sql-database-geo-replication-portal/create-secondary.png)
4. Optional können Sie einem elastischen Pool eine sekundäre Datenbank hinzufügen. Um die sekundäre Datenbank in einem Pool zu erstellen, klicken Sie auf **Elastischer Pool**, und wählen Sie einen Pool auf dem Zielserver aus. Ein Pool muss bereits auf dem Zielserver vorhanden sein. Dieser Workflow erstellt keinen Pool.
5. Klicken Sie auf **Erstellen** , um die sekundäre Datenbank hinzuzufügen.
6. Die sekundäre Datenbank wird erstellt, und der Seedingprozess beginnt.
   
    ![Konfigurieren der sekundären Datenbank](./media/sql-database-geo-replication-portal/seeding0.png)
7. Nach Abschluss des Seedingprozesses wird der Status für die zweite Datenbank angezeigt.
   
    ![Seeding abgeschlossen](./media/sql-database-geo-replication-portal/seeding-complete.png)

## <a name="remove-secondary-database"></a>Entfernen einer sekundären Datenbank 
Dieser Vorgang beendet die Replikation zur sekundären Datenbank dauerhaft und ändert die Rolle der sekundären Datenbank in eine normale Datenbank mit Lese-/Schreibzugriff. Wenn die Verbindung mit der sekundären Datenbank unterbrochen wird, ist der Befehl zwar erfolgreich, aber die sekundäre Datenbank wird erst mit Lese-/ Schreibzugriff versehen, nachdem die Verbindung wiederhergestellt wurde.  

1. Navigieren Sie im [Azure-Portal](http://portal.azure.com) zur primären Datenbank in der Georeplikationspartnerschaft.
2. Wählen Sie auf der Seite „SQL-Datenbank“ die Option **Georeplikation** aus.
3. Wählen Sie in der Liste **SEKUNDÄRE DATENBANKEN** die Datenbank aus, die Sie aus der Georeplikationspartnerschaft entfernen möchten.
4. Klicken Sie auf **Replikation beenden**.
   
    ![Sekundäre Datenbank entfernen](./media/sql-database-geo-replication-portal/remove-secondary.png)
5. Ein Bestätigungsfenster wird geöffnet. Klicken Sie zum Entfernen der Datenbank aus der Georeplikationspartnerschaft auf **Ja**. (Setzen Sie sie auf eine Datenbank mit Lese-/ Schreibzugriff fest, die kein Teil der Replikation ist.)

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zur aktiven Georeplikation finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).
* Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).


