---
title: 'Azure Cosmos DB-Portaltool: Abfrage-Explorer | Microsoft-Dokumentation'
description: "Sie erhalten Informationen zum Azure Cosmos DB-Abfrage-Explorer, einem SQL-Abfrage-Editor im Azure-Portal zum Schreiben von SQL-Abfragen und Ausführen dieser Abfragen für eine Azure Cosmos DB-Sammlung."
keywords: SQL-Abfragen schreiben, SQL-Abfrage-Editor
services: cosmosdb
author: kirillg
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: ac378240-b11f-4522-ae9f-09da3a6f9c16
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: kirillg
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 77389c6d0779a08b8d717076fc5678d3d7fb3b0c
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="write-edit-and-run-sql-queries-for-azure-cosmos-db-using-query-explorer-in-the-azure-portal"></a>Schreiben, Bearbeiten und Ausführen von SQL-Abfragen für Azure Cosmos DB mit dem Abfrage-Explorer im Azure-Portal
Dieser Artikel enthält eine Übersicht über den Abfrage-Explorer von [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/). Hierbei handelt es sich um ein Tool im Azure-Portal, mit dem Sie SQL-Abfragen für eine [DocumentDB-Sammlung](documentdb-create-collection.md) schreiben und bearbeiten und darauf anwenden können.

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) im linken Navigationsbereich auf ![Azure Cosmos DB-Symbol](./media/documentdb-query-collections-query-explorer/nosql-documentdb-portal-icon.png) **Azure Cosmos DB**. 

    Wenn **Azure Cosmos DB** nicht angezeigt wird, klicken Sie unten auf **Weitere Dienste** und dann auf ![Azure Cosmos DB-Symbol](./media/documentdb-query-collections-query-explorer/nosql-documentdb-portal-icon.png) **Azure Cosmos DB**.
2. Klicken Sie im Ressourcenmenü auf **Abfrage-Explorer**. 
   
    ![Screenshot: Azure-Portal mit hervorgehobenem Abfrage-Explorer](./media/documentdb-query-collections-query-explorer/queryexplorercommand.png)
3. Wählen Sie auf dem Blatt **Abfrage-Explorer** in den Dropdownlisten die **Datenbanken** und **Sammlungen** aus, die abgefragt werden sollen, und geben Sie die auszuführende Abfrage ein. 
   
    Die Dropdownlisten **Datenbanken** und **Sammlungen** werden basierend auf dem Kontext, in dem Sie den Abfrage-Explorer starten, automatisch ausgefüllt. 
   
    Die Standardabfrage `SELECT TOP 100 * FROM c` ist bereits vorhanden.  Sie können die Standardabfrage akzeptieren oder eine eigene Abfrage erstellen. Verwenden Sie hierfür die SQL-Abfragesprache, die unter [Spickzettel für SQL-Abfragen](documentdb-sql-query-cheat-sheet.md) bzw. im Artikel [SQL-Abfrage und SQL-Syntax](documentdb-sql-query.md) beschrieben wird.
   
    Klicken Sie auf **Abfrage ausführen** , um die Ergebnisse anzuzeigen.
   
    ![Screenshot: Schreiben von SQL-Abfragen im Abfrage-Explorer, SQL-Abfrage-Editor](./media/documentdb-query-collections-query-explorer/queryexplorerinitial.png)
4. Auf dem Blatt **Ergebnisse** wird die Ausgabe der Abfrage angezeigt. 
   
    ![Screenshot: Ergebnisse des Schreibens von SQL-Abfragen im Abfrage-Explorer](./media/documentdb-query-collections-query-explorer/queryresults1.png)

## <a name="work-with-results"></a>Arbeiten mit Ergebnissen
Standardmäßig zeigt der Abfrage-Explorer die Ergebnisse in Gruppen von jeweils 100 zurück.  Wenn Ihre Abfrage mehr als 100 Ergebnisse erzeugt, verwenden Sie einfach die Befehle **Nächste Seite** und **Vorherige Seite**, um durch die Ergebnisse zu navigieren.

![Screenshot des Abfrage-Explorers – Seitennummerierung](./media/documentdb-query-collections-query-explorer/queryresultspagination.png)

Um erfolgreiche Abfragen zu erhalten, enthält der Bereich **Informationen** Kennzahlen, z.B. die Anforderungskosten, die Anzahl von Roundtrips der Abfrage und den aktuell angezeigten Ergebnissatz. Sie erfahren auch, ob weitere Ergebnisse vorhanden sind, die – wie bereits erwähnt – über den Befehl **Nächste Seite** angezeigt werden können.

![Screenshot der Abfrage-Explorers – Abfrageinformationen](./media/documentdb-query-collections-query-explorer/queryinformation.png)

## <a name="use-multiple-queries"></a>Verwenden mehrerer Abfragen
Wenn Sie mehrere Abfragen verwenden und schnell zwischen den Abfragen wechseln möchten, können Sie alle Abfragen im Blatt **Abfrage-Explorer** in das Abfragetextfeld eingeben und dann die Abfrage markieren, die Sie ausführen möchten. Klicken Sie anschließend auf **Abfrage ausführen**, um die Ergebnisse anzuzeigen.

![Screenshot: Schreiben mehrerer SQL-Abfragen im Abfrage-Explorer (SQL-Abfrage-Editor) und Markieren und Ausführen einzelner Abfragen](./media/documentdb-query-collections-query-explorer/queryexplorerhighlightandrun.png)

## <a name="add-queries-from-a-file-into-the-sql-query-editor"></a>Hinzufügen von Abfragen aus einer Datei in den SQL-Abfrage-Editor
Mithilfe des Befehls **Datei laden** können Sie die Inhalte einer vorhandenen Datei laden.

![Screenshot: Laden von SQL-Abfragen aus einer Datei in den Abfrage-Explorer per „Datei laden“](./media/documentdb-query-collections-query-explorer/loadqueryfile.png)

## <a name="troubleshoot"></a>Problembehandlung
Wenn eine Abfrage mit Fehlern abgeschlossen wird, zeigt der Abfrage-Explorer eine Liste der Fehler an, die Sie bei der Fehlerbehebung unterstützen.

![Screenshot der Abfrage-Explorers – Abfragefehler](./media/documentdb-query-collections-query-explorer/queryerror.png)

## <a name="run-documentdb-api-sql-queries-outside-the-portal"></a>Ausführen von DocumentDB-API-SQL-Abfragen außerhalb des Portals
Der Abfrage-Explorer im Azure-Portal ist nur eine Möglichkeit, um SQL-Abfragen für Cosmos DB auszuführen. Sie können SQL-Abfragen auch mit der [REST-API](https://msdn.microsoft.com/library/azure/dn781481.aspx) oder den [Client-SDKs](documentdb-sdk-dotnet.md) ausführen. Weitere Informationen zur Verwendung dieser oder anderer Methoden finden Sie unter [Ausführen von SQL-Abfragen](documentdb-sql-query.md#ExecutingSqlQueries)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur DocumentDB-API-SQL-Grammatik, die im Abfrage-Explorer unterstützt wird, finden Sie im Artikel [SQL-Abfrage und SQL-Syntax](documentdb-sql-query.md), oder drucken Sie sich den [Spickzettel für SQL-Abfragen](documentdb-sql-query-cheat-sheet.md) aus.
Sie können auch experimentieren, indem Sie auf den [Query Playground](https://www.documentdb.com/sql/demo) zugreifen, um Abfragen online mit einem Beispieldataset zu testen.


