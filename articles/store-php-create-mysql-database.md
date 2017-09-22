---
title: Erstellen einer MySQL-Datenbank und Herstellen einer Verbindung in Azure
description: "Hier erfahren Sie, wie Sie über das Azure-Portal eine MySQL-Datenbank erstellen und anschließend über eine PHP-Web-App in Azure eine Verbindung mit dieser Datenbank herstellen."
documentationcenter: php
services: app-service\web
author: cephalin
manager: erikre
editor: 
tags: mysql
ms.assetid: 55465a9a-7e65-4fd9-8a65-dd83ee41f3e5
ms.service: multiple
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm;cephalin
ms.translationtype: HT
ms.sourcegitcommit: 8f9234fe1f33625685b66e1d0e0024469f54f95c
ms.openlocfilehash: c072cb3a7d376d1e3c2b9f741f5410106e701256
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---
# <a name="create-and-connect-to-a-mysql-database-in-azure"></a>Erstellen einer MySQL-Datenbank und Herstellen einer Verbindung in Azure
In diesem Tutorial erfahren Sie, wie Sie über das [Azure-Portal](https://portal.azure.com) (Anbieter: [ClearDB](http://www.cleardb.com/)) eine MySQL-Datenbank erstellen und über eine in [Azure App Service](app-service/app-service-web-overview.md) ausgeführte PHP-Web-App eine Verbindung mit der Datenbank herstellen.

> [!NOTE]
> Sie können auch eine MySQL-Datenbank auf der Grundlage einer <a href="https://portal.azure.com/#create/WordPress.WordPress" target="_blank">Marketplace-App-Vorlage</a>erstellen.
>
>

## <a name="create-a-mysql-database-in-azure-portal"></a>Erstellen einer MySQL-Datenbank über das Azure-Portal
Gehen Sie wie folgt vor, um eine MySQL-Datenbank über das Azure-Portal zu erstellen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie im linken Menü auf **Neu** > **Daten und Speicher** > **MySQL-Datenbank**.

    ![Erstellen einer MySQL-Datenbank in Azure – Start](./media/store-php-create-mysql-database/create-db-1-start.png)
3. Konfigurieren Sie Ihre neue MySQL-Datenbank auf dem [Blatt](azure-portal-overview.md) „MySQL-Datenbank“ wie im Anschluss beschrieben. (*Blatt*: eine Portalseite, die horizontal geöffnet wird.)

   * **Datenbankname**: Geben Sie einen eindeutigen Namen ein.
   * **Abonnement**: Wählen Sie das zu verwendende Abonnement aus.
   * **Datenbanktyp**: Wählen Sie **Shared** (kostengünstige/kostenlose Tarife) oder **Dediziert** (dedizierte Ressourcen) aus.
   * **Ressourcengruppe**: Fügen Sie die MySQL-Datenbank einer bereits vorhandenen oder einer neuen [Ressourcengruppe](azure-resource-manager/resource-group-overview.md) hinzu. Ressourcen, die sich in der gleichen Gruppe befinden, lassen sich komfortabel gemeinsam verwalten.
   * **Standort**: Wählen Sie einen Standort in Ihrer Nähe aus. Beim Hinzufügen zu einer bereits vorhandenen Ressourcengruppe ist als Standort der Standort dieser Ressourcengruppe vorgegeben.
   * **Tarif**: Klicken Sie auf **Tarif**, wählen Sie eine Preisoption aus (**Mercury** ist kostenlos), und klicken Sie anschließend auf **Auswählen**.
   * **Rechtliche Bedingungen**: Klicken Sie auf **Rechtliche Bedingungen**, überprüfen Sie die Details der Bestellung, und klicken Sie auf **Kaufen**.
   * **An Dashboard anheften**: Wählen Sie aus, ob Sie direkt über das Dashboard darauf zugreifen möchten. Dies ist besonders hilfreich, wenn Sie noch nicht mit der Portalnavigation vertraut sind.

     Der folgende Screenshot zeigt eine Beispielkonfiguration für eine MySQL-Datenbank:  
     ![Erstellen einer MySQL-Datenbank in Azure – Konfigurieren](./media/store-php-create-mysql-database/create-db-2-configure.png)
4. Klicken Sie nach Abschluss der Konfiguration auf **Erstellen**.

    ![Erstellen einer MySQL-Datenbank in Azure – Erstellen](./media/store-php-create-mysql-database/create-db-3-create.png)

    Eine Popupbenachrichtigung informiert darüber, dass die Bereitstellung gestartet wurde.

    ![Erstellen einer MySQL-Datenbank in Azure – in Bearbeitung](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    Die erfolgreiche Bereitstellung wird durch eine weitere Popupbenachrichtigung signalisiert. Im Portal wird außerdem automatisch das Blatt für Ihre MySQL-Datenbank geöffnet.

<a name="connect"></a>

## <a name="connect-to-your-mysql-database"></a>Herstellen von Verbindungen mit der MySQL-Datenbank
Klicken Sie zum Anzeigen der Verbindungsinformationen für Ihre neue MySQL-Datenbank auf dem Blatt Ihrer Web-App einfach auf **Eigenschaften** .

![Erstellen einer MySQL-Datenbank in Azure – Blatt „MySQL-Datenbank“](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

Die Verbindungsinformationen können in jeder beliebigen Web-App verwendet werden. Ein Beispiel zur Veranschaulichung der Verwendung von Verbindungsinformationen in einer einfachen PHP-App finden Sie [hier](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie im [PHP Developer Center](/develop/php/).

