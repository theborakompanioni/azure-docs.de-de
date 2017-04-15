---
title: "Azure-Portal: Dynamische Datenmaskierung für SQL-Datenbank | Microsoft Docs"
description: "Einstieg in die dynamische Datenmaskierung für SQL-Datenbank im Azure-Portal"
services: sql-database
documentationcenter: 
author: ronitr
manager: jhubbard
editor: 
ms.assetid: 2
ms.service: sql-database
ms.custom: security-protect
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 11/22/2016
ms.author: ronitr; ronmat
translationtype: Human Translation
ms.sourcegitcommit: 07913359addaa49fe8d408a8e363f554419e4de2
ms.openlocfilehash: 16db41c83ca5c582aca7581ae9b6a03a3d99ab15
ms.lasthandoff: 02/16/2017


---
# <a name="get-started-with-sql-database-dynamic-data-masking-with-the-azure-portal"></a>Erste Schritte mit der dynamischen Datenmaskierung für SQL-Datenbank im Azure-Portal

In diesem Thema wird erläutert, wie die [dynamische Datenmaskierung](sql-database-dynamic-data-masking-get-started.md) im Azure-Portal implementiert wird. Die dynamische Datenmaskierung können Sie auch mithilfe von [Azure SQL-Datenbank-Cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx) oder der [REST-API](https://msdn.microsoft.com/library/dn505719.aspx) implementieren.


## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-portal"></a>Einrichten der dynamischen Datenmaskierung für Ihre Datenbank im Azure-Portal
1. Rufen Sie das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com)auf.
2. Navigieren Sie zum Blatt mit den Einstellungen der Datenbank mit den sensiblen Daten, die Sie maskieren möchten.
3. Klicken Sie auf die Kachel **Dynamische Datenmaskierung**, woraufhin das Konfigurationsblatt **Dynamische Datenmaskierung** geöffnet wird.
   
   * Alternativ können Sie nach unten zum Abschnitt **Vorgänge** scrollen und auf **Dynamische Datenmaskierung** klicken.
     
     ![Navigationsbereich](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>
4. Auf dem Konfigurationsblatt **Dynamische Datenmaskierung** werden ggf. einige Datenbankspalten angezeigt, die vom Empfehlungsmodul für die Maskierung gekennzeichnet wurden. Klicken Sie zum Akzeptieren der Empfehlungen einfach für eine oder mehrere Spalten auf **Maske hinzufügen**. Es wird basierend auf dem Standardtyp der jeweiligen Spalte eine Maskierung erstellt. Sie können die Maskierungsfunktion ändern, indem Sie auf die Maskierungsregel klicken und das Maskierungsfeldformat in ein anderes Format Ihrer Wahl ändern. Klicken Sie auf **Speichern**, um Ihre Einstellungen zu speichern.
   
    ![Navigationsbereich](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>
5. Klicken Sie zum Hinzufügen einer Maskierung für eine beliebige Spalte der Datenbank oben auf dem Konfigurationsblatt **Dynamische Datenmaskierung** auf **Maske hinzufügen**, um das Konfigurationsblatt **Maskierungsregel hinzufügen** zu öffnen.
   
    ![Navigationsbereich](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>
6. Wählen Sie **Schema**, **Tabelle** und **Spalte** aus, um das Feld zu bestimmen, das maskiert werden soll.
7. Wählen Sie in der Liste der Kategorien für das Maskieren empfindlicher Daten ein **Maskierungsfeldformat** .
   
    ![Navigationsbereich](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>        
8. Klicken Sie auf dem Datenmaskierungsregel-Blatt auf **Speichern** , um die Gruppe von Maskierungsregeln in der Richtlinie für die dynamische Datenmaskierung zu aktualisieren.
9. Geben Sie die SQL-Benutzer oder AAD-Identitäten ein, die von der Maskierung ausgeschlossen werden sollen und Zugriff auf die vertraulichen Daten ohne Maskierung haben. Hierbei sollte es sich um eine durch Semikolons getrennte Liste mit Benutzern handeln. Beachten Sie, dass Benutzer mit Administratorrechten immer Zugriff auf die Originaldaten ohne Maskierung haben.
   
    ![Navigationsbereich](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)
   
   > [!TIP]
   > Damit die Anwendungsschicht sensible Daten für die privilegierten Anwendungsbenutzer anzeigen kann, fügen Sie den SQL-Benutzer oder die AAD-Identität hinzu, die von der Anwendung zum Abfragen der Datenbank verwendet wird. Es wird ausdrücklich empfohlen, dass diese Liste nur eine minimale Anzahl von privilegierten Benutzern enthält, um die Anzeige sensibler Daten zu minimieren.
   > 
   > 
10. Klicken Sie auf dem Konfigurationsblatt für die Datenmaskierung auf **Speichern** , um die neue oder aktualisierte Maskierungsrichtlinie zu speichern.


## <a name="next-steps"></a>Nächste Schritte

* Eine Übersicht über die dynamische Datenmaskierung finden Sie unter [Dynamische Datenmaskierung](sql-database-dynamic-data-masking-get-started.md).
* Die dynamische Datenmaskierung können Sie auch mithilfe von [Azure SQL-Datenbank-Cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx) oder der [REST-API](https://msdn.microsoft.com/library/dn505719.aspx) implementieren.
