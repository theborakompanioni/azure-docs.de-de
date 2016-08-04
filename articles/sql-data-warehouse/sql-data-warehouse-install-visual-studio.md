<properties
   pageTitle="Installieren von Visual Studio und SSDT für SQL Data Warehouse | Microsoft Azure"
   description="Installieren von Visual Studio und SQL Server Data Tools (SSDT) für Azure SQL Data Warehouse"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/23/2016"
   ms.author="sonyama;barbkess"/>

# Installieren von Visual Studio 2015 und SSDT für SQL Data Warehouse

Für die Entwicklung von Anwendungen für SQL Data Warehouse empfiehlt es sich, Visual Studio 2015 in Verbindung mit der neuesten Version von SQL Server Data Tools (SSDT) zu verwenden. Visual Studio 2013 Update 5 mit SSDT wird auch für die Abwärtskompatibilität unterstützt.

Die Verwendung von Visual Studio mit SSDT ermöglicht die Nutzung des SQL Server-Objekt-Explorers zum Visualisieren von Tabellen, Ansichten, gespeicherten Prozeduren und zahlreichen weiteren Objekten in SQL Data Warehouse sowie zum Ausführen von Abfragen.

> [AZURE.NOTE] SQL Data Warehouse unterstützt noch keine Visual Studio-Datenbankprojekte. Dieses Feature wird in einer späteren Version hinzugefügt.

## Schritt 1: Installieren von Visual Studio 2015

Verwenden Sie die folgenden Links, um Visual Studio 2015 herunterzuladen und zu installieren. Falls Sie Visual Studio 2013 oder 2015 bereits installiert haben, fahren Sie mit Schritt 2 fort, um SSDT zu installieren.

1. [Laden Sie Visual Studio 2015 herunter][].
2. Führen Sie die Schritte der auf MSDN bereitgestellten Anleitung [Installieren von Visual Studio][] aus, und wählen Sie die Standardkonfigurationen aus.

## Schritt 2: Installieren von SSDT

Führen Sie zum Installieren von SSDT für Visual Studio einfach die folgenden Schritte aus, um in Visual Studio nach einem SSDT-Update zu suchen:

1. Klicken Sie in Visual Studio auf **Extras** > **Erweiterungen und Updates...** > **Updates**.
2. Wählen Sie **Produktupdates**, und suchen Sie nach **Microsoft SQL Server-Update für Datenbanktools**.

Wird kein Update gefunden, ist bereits die neueste Version installiert. Klicken Sie auf **Hilfe** > **Info**, und suchen Sie in der Liste nach „SQL Server Data Tools“, um sich zu vergewissern, dass SSDT installiert ist. Die neueste Version von SSDT ist 14.0.60525.0. Falls die Option zur Installation in Visual Studio nicht verfügbar ist, können Sie SSDT auf der Seite für den [SSDT-Download][] manuell herunterladen und installieren.

## Nächste Schritte

Da Sie jetzt die neueste Version von SSDT verwenden, sind Sie bereit, eine [Verbindung][] mit SQL Data Warehouse herzustellen.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[Verbindung]: ./sql-data-warehouse-get-started-connect.md

<!--Other-->
[Laden Sie Visual Studio 2015 herunter]: https://www.visualstudio.com/downloads/
[Installieren von Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT-Download]: https://msdn.microsoft.com/library/mt204009.aspx

<!---HONumber=AcomDC_0727_2016-->