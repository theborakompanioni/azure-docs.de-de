---
title: "Gewusst wie: Erstellen eines Supporttickets für SQL Data Warehouse | Microsoft Docs"
description: Sie erfahren, wie Sie in Azure SQL Data Warehouse ein Supportticket erstellen.
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: a91d1f53-03cb-464b-9d5b-4a9c1a194ed3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 889776269aa3d722115ea0632b7c3db2775924eb
ms.openlocfilehash: cc3bc8a0463842ba11b9cdcc996319e2d811ce8a
ms.lasthandoff: 02/17/2017


---
# <a name="how-to-create-a-support-ticket-for-sql-data-warehouse"></a>Gewusst wie: Erstellen eines Supporttickets für SQL Data Warehouse
Bei Problemen mit Ihrer Instanz von SQL Data Warehouse ist es ratsam, ein Supportticket zu erstellen, damit Sie Hilfe von unserem Entwicklungsteam erhalten können.

> [!NOTE] 
> Seit dem 20.12.2016 funktioniert die Ressourcenintegritätsüberprüfung im Azure-Portal nicht mehr ordnungsgemäß. Wir arbeiten aktiv an der Behebung dieses Problems. 


## <a name="create-a-support-ticket"></a>Erstellen ein Supporttickets
1. Öffnen Sie das [Azure-Portal][Azure portal].
2. Klicken Sie auf der Startseite auf die Kachel **Hilfe und Support** .
   
    ![Hilfe und Support](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)
3. Klicken Sie auf dem Blatt „Hilfe und Support“ auf **Supportanfrage erstellen**.
   
    ![Neue Supportanfrage](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
   
    <a name="request-quota-change"></a> 
4. Wählen Sie den **Typ der Anfrage**aus.
   
    ![Typ der Anfrage](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
   
   > [!NOTE]
   > Standardmäßig verfügt jeder SQL-Server (z.B. myserver.database.windows.net) über ein **DTU-Kontingent** in Höhe von 45.000. Bei diesem Kontingentwert handelt es sich einfach um ein Sicherheitslimit. Sie können Ihr Kontingent erhöhen, indem Sie ein Supportticket erstellen und als Anfragetyp *Kontingent* wählen. Multiplizieren Sie zum Berechnen Ihrer DTU-Anforderungen die Anzahl der insgesamt benötigten [DWUs][DWU] mit 7,5. Wenn Sie beispielsweise zwei DW6000-Einheiten auf einer SQL Server-Instanz hosten möchten, sollten Sie ein DTU-Kontingent in Höhe von 90.000 anfordern.  Sie können den aktuellen DTU-Verbrauch im Portal auf dem Blatt „SQL-Server“ anzeigen. Sowohl angehaltene als auch nicht angehaltene Datenbanken werden in das DTU-Kontingent eingerechnet. 
   > 
   > 
5. Wählen Sie das **Abonnement** aus, unter dem die Datenbank mit dem gemeldeten Problem gehostet wird.
   
    ![Abonnement](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)
6. Wählen Sie **SQL Data Warehouse** als Ressource aus.
   
    ![Ressource](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)
7. Wählen Sie Ihren [Azure-Supportplan][Azure support plan].
   
   * **Verwaltung von Abrechnung, Kontingenten und Abonnements** ist in allen Supportstufen verfügbar.
   * **Problemlösungssupport** wird in den Supportstufen [Entwickler][Developer], [Standard][Standard], [Professional Direct][Professional Direct] oder [Premier][Premier] bereitgestellt. Problemlösungssupport betrifft Probleme, die bei der Verwendung von Azure auftreten und bei denen Grund zu der Annahme besteht, dass das Problem durch Microsoft verursacht wurde.
   * **Anleitung für Entwickler** und **beratende Dienstleistungen** sind für die Supportstufen [Professional Direct][Professional Direct] und [Premier][Premier] verfügbar. 
     
     Wenn Sie über den Supportplan „Premier“ verfügen, können Sie Probleme im Zusammenhang mit SQL Data Warehouse auch im [Microsoft Premier-Onlineportal][Microsoft Premier online portal] melden.  Weitere Informationen zu den unterschiedlichen Supportplänen, z.B. zu Umfang, Reaktionszeiten, Preisen usw., finden Sie auf der Seite mit den [Azure-Supportplänen][Azure support plan].  Häufig gestellte Fragen zum Azure-Support finden Sie unter [Häufig gestellte Fragen zum Azure-Support][Azure support FAQs].  
     
     ![Supportplan](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)
8. Wählen Sie **Problemtyp** und **-kategorie** aus. In diesem Beispiel haben wir „Tools“ als Problemtyp und „Client tools“ als Kategorie gewählt. 
   
    ![Problemtypkategorie](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)
9. Beschreiben Sie das Problem, und wählen Sie den Grad der geschäftlichen Auswirkungen aus.
   
    ![Problembeschreibung](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)
10. Ihre **Kontaktinformationen** sind für dieses Supportticket bereits vorhanden. Aktualisieren Sie diese bei Bedarf.
    
    ![Kontaktinformationen](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)
11. Klicken Sie auf **Erstellen** , um die Supportanfrage zu übermitteln.

## <a name="monitor-a-support-ticket"></a>Überwachen eines Supporttickets
Nachdem Sie die Supportanfrage übermittelt haben, nimmt das Azure-Supportteam Kontakt mit Ihnen auf. Klicken Sie zum Überprüfen des Anfragenstatus und der Details auf dem Dashboard auf **Supportanfragen verwalten** .

![Status überprüfen](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a>Weitere Ressourcen
Außerdem können Sie sich an der SQL Data Warehouse-Community unter [Stack Overflow][Stack Overflow] oder im [Azure SQL Data Warehouse-MSDN-Forum][Azure SQL Data Warehouse MSDN forum] beteiligen.

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md

<!--MSDN references--> 

<!--Other web references--> 
[Azure portal]: https://portal.azure.com/
[Azure support plan]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Developer]: https://azure.microsoft.com/support/plans/developer/  
[Standard]: https://azure.microsoft.com/support/plans/standard/  
[Professional Direct]: https://azure.microsoft.com/support/plans/prodirect/  
[Premier]: https://azure.microsoft.com/support/plans/premier/  
[Azure support FAQs]: https://azure.microsoft.com/support/faq/
[Microsoft Premier online portal]: https://premier.microsoft.com/
[Stack Overflow]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[Azure SQL Data Warehouse MSDN forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/


