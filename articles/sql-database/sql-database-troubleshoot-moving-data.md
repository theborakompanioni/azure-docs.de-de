---
title: Informationen zum Verschieben von Datenbanken zwischen Servern und Abonnements sowie in und aus Azure.
description: Kurzanweisungen zum Kopieren, Verschieben und Migrieren von Daten und Datenbanken in Azure SQL-Datenbank.
services: sql-database
documentationcenter: 
author: v-shysun
manager: felixwu
editor: 
ms.assetid: dea01868-5757-41e2-826c-ec26361a339c
ms.service: sql-database
ms.custom: migrate and move
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: v-shysun
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: bc88bb49c601f525c62c4dac4716a3f77a30172e


---
# <a name="move-databases-between-servers-between-subscriptions-and-in-and-out-of-azure"></a>Verschieben von Datenbanken zwischen Servern und Abonnements sowie in und aus Azure
[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="to-move-a-database-to-a-different-server-in-the-same-subscription"></a>So verschieben Sie eine Datenbank auf einen anderen Server im selben Abonnement
* Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **SQL-Datenbanken**, wählen Sie in der Liste eine Datenbank aus, und klicken Sie dann auf **Kopieren**. Weitere Details finden Sie unter [Kopieren einer Azure SQL-Datenbank](sql-database-copy.md) .

## <a name="to-move-a-database-between-subscriptions"></a>So verschieben Sie eine Datenbank zwischen Abonnements
* Klicken Sie im [Azure-Portal](https://portal.azure.com)auf **SQL-Server** , und wählen Sie in der Liste den Server aus, der die Datenbank hostet. Klicken Sie auf **Verschieben**, und wählen Sie die zu verschiebenden Ressourcen und das Abonnement aus, in das dieser Vorgang erfolgen soll.

## <a name="to-migrate-a-sql-database-into-azure"></a>So migrieren Sie eine SQL-Datenbank in Azure
* Bestimmen Sie die Datenbankkompatibilität, und wählen Sie dann basierend auf Ihren Anforderungen die geeignete Migrationsmethode. Befolgen Sie die Anleitungen und Optionen unter [Migrieren einer SQL Server-Datenbank](sql-database-cloud-migrate.md).

## <a name="to-create-a-copy-of-a-database-for-use-outside-of-azure"></a>So erstellen Sie eine Kopie einer Datenbank zur Verwendung außerhalb von Azure
* [Exportieren Sie eine BACPAC-Datei.](sql-database-export.md)




<!--HONumber=Nov16_HO3-->


