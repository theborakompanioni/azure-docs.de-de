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
   ms.date="05/16/2016"
   ms.author="sonyama;barbkess"/>

# Installieren von Visual Studio 2015 und SSDT für SQL Data Warehouse

Für die Entwicklung von Anwendungen für SQL Data Warehouse empfiehlt es sich, Visual Studio 2015 in Verbindung mit der neuesten Version von SQL Server Data Tools (SSDT) zu verwenden. Visual Studio 2013 mit SSDT wird ebenfalls unterstützt.

Darüber hinaus wird **Microsoft SQL Server-Update für Datenbanktools** benötigt, um Abfragen über die integrierte Visual Studio-Entwicklungsumgebung (Integrated Development Environment, IDE) ausführen zu können. Wenn diese Erweiterung installiert ist, können Sie Datenbankobjekte in der Struktur des Objekt-Explorers anzeigen und Abfragen für Ihr SQL Data Warehouse ausführen.

> [AZURE.NOTE] SQL Data Warehouse unterstützt noch keine Visual Studio-Datenbankprojekte. Dieses Feature wird in einer späteren Version hinzugefügt.

## Schritt 1: Installieren von Visual Studio 2015

Verwenden Sie die folgenden Links, um Visual Studio 2015 herunterzuladen und zu installieren. Falls Sie Visual Studio 2013 oder 2015 bereits installiert haben, fahren Sie mit Schritt 2 fort, um SSDT zu installieren.

1. [Laden Sie Visual Studio 2015][] aus Visual Studio Team Services herunter.
2. Führen Sie die Schritte der auf MSDN bereitgestellten Anleitung [Installieren von Visual Studio][] durch, und wählen Sie die Standardkonfigurationen aus.

## Schritt 2: Installieren von SSDT

Führen Sie zum Installieren von SSDT für Visual Studio einfach die folgenden Schritte aus, um in Visual Studio nach einem SSDT-Update zu suchen:

1. Klicken Sie in Visual Studio auf **Extras** > **Erweiterungen und Updates...** > **Updates**.
2. Wählen Sie **Produktupdates** aus, und suchen Sie nach **Microsoft SQL Server-Update für Datenbanktools**.

Wird kein Update gefunden, ist bereits die neueste Version installiert. Klicken Sie auf **Hilfe** > **Info**, und suchen Sie in der Liste nach „SQL Server Data Tools“, um sich zu vergewissern, dass SSDT installiert ist.

## Nächste Schritte

Da Sie jetzt die neueste Version von SSDT verwenden, sind Sie bereit, eine [Verbindung][] mit der Datenbank herzustellen.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[Verbindung]: ./sql-data-warehouse-get-started-connect.md

<!--Other-->
[Laden Sie Visual Studio 2015]: https://www.visualstudio.com/downloads/
[Installieren von Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx

<!---HONumber=AcomDC_0518_2016-->