---
title: Verwenden von Azure Stream Analytics mit SQL Data Warehouse | Microsoft Docs
description: "Tipps für die Verwendung von Azure Stream Analytics mit Azure SQL Data Warehouse für die Entwicklung von Lösungen."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: barbkess
editor: 
ms.assetid: 8aeb2247-20c5-4a29-b327-30a8ce09dfdc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: kevin;barbkess
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: d1c328ec609cc27a42fb6b30897d7f0b466134e7
ms.lasthandoff: 04/03/2017


---
# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a>Verwenden von Azure Stream Analytics mit SQL Data Warehouse
Azure Stream Analytics ist ein vollständig verwalteter Dienst, der eine geringe Latenz, hohe Verfügbarkeit und eine skalierbare komplexe Ereignisverarbeitung durch das Streaming von Daten in der Cloud bietet. Die Grundlagen finden Sie unter [Einführung in Azure Stream Analytics][Introduction to Azure Stream Analytics]. In [Erste Schritte mit Azure Stream Analytics][Get started using Azure Stream Analytics] können Sie sich dann mit dem Erstellen einer End-to-End-Lösung mit Stream Analytics vertraut machen.

In diesem Artikel erfahren Sie, wie Sie die Azure SQL Data Warehouse-Datenbank als Ausgabesenke für Stream Analytics-Aufgaben verwenden können.

## <a name="prerequisites"></a>Voraussetzungen
Führen Sie zuerst die folgenden Schritte im Lernprogramm [Erste Schritte mit Azure Stream Analytics][Get started using Azure Stream Analytics] aus.  

1. Erstellen einer Event Hub-Eingabe
2. Konfigurieren und starten der Ereignisgenerator-Anwendung
3. Bereitstellen eines Stream Analytics-Auftrags
4. Festlegen der Auftragseingabe und -abfrage

Erstellen Sie dann eine Azure SQL Data Warehouse-Datenbank.

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>Festlegen der Auftragsausgabe: Azure SQL Data Warehouse-Datenbank
### <a name="step-1"></a>Schritt 1
Klicken Sie in Ihrem Stream Analytics-Auftrag am oberen Rand der Seite auf **AUSGABE**, und klicken Sie anschließend auf **AUSGABE HINZUFÜGEN**.

### <a name="step-2"></a>Schritt 2
Wählen Sie „SQL-Datenbank“ aus, und klicken Sie auf „Weiter“.

![][add-output]

### <a name="step-3"></a>Schritt 3
Geben Sie auf der nächsten Seite die folgenden Werte ein:

* *Ausgabealias*: Geben Sie einen Anzeigenamen für diese Auftragsausgabe ein.
* *Abonnement*:
  * Befindet sich die SQL Data Warehouse-Datenbank im selben Abonnement wie der Stream Analytics-Auftrag, wählen Sie die Option "SQL-Datenbank aus aktuellem Abonnement verwenden" aus.
  * Wenn die Datenbank sich in einem anderen Abonnement befindet, wählen Sie "SQL-Datenbank aus einem anderen Abonnement verwenden" aus.
* *Datenbank*: Geben Sie den Namen einer Zieldatenbank an.
* *Servername*: Geben Sie den Servernamen für die Datenbank an, die Sie soeben angegeben haben. Diese Angaben finden Sie im klassischen Azure-Portal.

![][server-name]

* *Benutzername*: Geben Sie den Benutzernamen eines Kontos mit Schreibberechtigungen für die Datenbank an.
* *Kennwort*: Geben Sie das Kennwort für das angegebene Benutzerkonto an.
* *Tabelle*: Geben Sie den Namen der Zieltabelle in der Datenbank an.

![][add-database]

### <a name="step-4"></a>Schritt 4
Klicken Sie auf das Häkchen, um diese Auftragsausgabe hinzuzufügen und um zu überprüfen, ob Stream Analytics erfolgreich mit der Datenbank verbunden werden kann.

![][test-connection]

Wenn die Verbindung mit der Datenbank hergestellt wird, wird unten im Portal eine entsprechende Meldung angezeigt. Sie können dann unten auf "Verbindung testen" klicken, um die Verbindung mit der Datenbank zu testen.

## <a name="next-steps"></a>Nächste Schritte
Einen Überblick über die Integration finden Sie unter [SQL Data Warehouse-Integration (Übersicht)][SQL Data Warehouse integration overview].

Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht für SQL Data Warehouse][SQL Data Warehouse development overview].

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Introduction to Azure Stream Analytics]: ../stream-analytics/stream-analytics-introduction.md
[Get started using Azure Stream Analytics]: ../stream-analytics/stream-analytics-get-started.md
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/

