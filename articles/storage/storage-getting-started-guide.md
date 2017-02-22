---
title: "Erste Schritte mit Azure Storage – Einstieg in fünf Minuten | Microsoft Docs"
description: "Finden Sie schnell den Einstieg in Microsoft Azure-Blobs, -Tabellen und -Warteschlangen mithilfe von Azure Storage-Schnellstarts, Visual Studio und dem Azure-Speicheremulator. Führen Sie innerhalb von fünf Minuten Ihre erste Azure Storage-Anwendung aus."
services: storage
documentationcenter: .net
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 582f76f8-c814-4a69-8a5c-1fd0e0d5d8f2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 12/08/2016
ms.author: marsma
translationtype: Human Translation
ms.sourcegitcommit: 12ce6b6bccf3ea2aa2945ddd775716f29cf01e1f
ms.openlocfilehash: 47b2623eb3b83220ef8e3cfafde06dab3ac3d22e


---
# <a name="get-started-with-azure-storage-in-five-minutes"></a>Erste Schritte mit Azure Storage – Einstieg in fünf Minuten
## <a name="overview"></a>Übersicht
Der Einstieg in die Entwicklung mit Azure Storage ist einfach. In diesem Lernprogramm erfahren Sie, wie Ihre Anwendungen in Azure Storage in kürzester Zeit betriebsbereit sind. Sie verwenden die Schnellstartvorlagen, die im Azure SDK für .NET enthalten sind. Diese Vorlagen enthalten Code, der sofort ausgeführt werden kann und einige grundlegende Programmierungsszenarien in Azure Storage demonstriert.

Wenn Sie mehr über Azure Storage erfahren möchten, bevor Sie am Code arbeiten, finden Sie die entsprechenden Informationen unter [Nächste Schritte](#next-steps).

## <a name="prerequisites"></a>Voraussetzungen
Folgende Voraussetzungen müssen erfüllt sein, bevor Sie beginnen:

1. Zum Kompilieren und Erstellen der Anwendung muss [Visual Studio](https://www.visualstudio.com/) auf Ihrem Computer installiert sein.
2. Installieren Sie die neueste Version von [Azure SDK für .NET](https://azure.microsoft.com/downloads/). Das SDK enthält die Azure-Schnellstart-Beispielprojekte, den Azure-Speicheremulator und die [Azure Storage Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).
3. Stellen Sie sicher, dass [.NET Framework 4.5](http://www.microsoft.com/download/details.aspx?id=30653) auf Ihrem Computer installiert ist, da es von den Azure-Schnellstart-Beispielprojekte benötigt wird, die wir in diesem Lernprogramm verwenden.

    Wenn Sie nicht sicher sind, welche Version von .NET Framework auf dem Computer installiert ist, finden Sie entsprechende Informationen unter [Gewusst wie: Feststellen, welche Versionen von .NET Framework installiert sind](https://msdn.microsoft.com/vstudio/hh925568.aspx). Klicken Sie auf die Schaltfläche **Start**, oder drücken Sie die Windows-Taste, und geben Sie **Systemsteuerung** ein. Klicken Sie auf **Programme** > **Programme und Funktionen**, und ermitteln Sie, ob .NET Framework 4.5 unter den installierten Programme aufgeführt ist.
4. Sie benötigen ein Azure-Abonnement und ein Azure-Speicherkonto.

   * Informationen zum Erhalt eines Azure-Abonnements finden Sie unter [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/), [Kaufoptionen](https://azure.microsoft.com/pricing/purchase-options/) und [Angebote für Mitglieder](https://azure.microsoft.com/pricing/member-offers/) (für Mitglieder von MSDN, Microsoft Partner Network, BizSpark und anderen Microsoft-Programmen).
   * Informationen zum Erstellen eines Speicherkontos in Azure finden Sie unter [Erstellen eines Speicherkontos](storage-create-storage-account.md#create-a-storage-account).

## <a name="run-your-first-azure-storage-application-against-azure-storage-in-the-cloud"></a>Ausführen Ihrer ersten Azure Storage-Anwendung über Azure Storage in der Cloud
Sobald das Konto eingerichtet ist, können Sie mithilfe eines der Azure-Schnellstart-Beispielprojekte in Visual Studio eine einfache Azure Storage-Anwendung erstellen. Dieses Tutorial konzentriert sich auf die Beispielprojekte für Azure Storage: **Azure Storage: Blobs**, **Azure Storage: Dateien**, **Azure Storage: Warteschlangen** und **Azure Storage: Tabellen**:

1. Starten Sie Visual Studio.
2. Klicken Sie im Menü **Datei** auf **Neues Projekt**.
3. Klicken Sie im Dialogfeld **Neues Projekt** auf **Installiert** > **Vorlagen** > **Visual C#** > **Cloud** > **Schnellstarts** > **Data Services**.
    a. Wählen Sie eine der folgenden Vorlagen aus: **Azure Storage: Blobs**, **Azure Storage: Dateien**, **Azure Storage: Warteschlangen** oder **Azure Storage: Tabellen**.
    b. Stellen Sie sicher, dass als Zielframework **.NET Framework 4.5** ausgewählt ist.
    c. Geben Sie einen Namen für das Projekt an, und erstellen Sie die neue Visual Studio-Projektmappe, wie hier zu sehen:

    ![Azure-Schnellstarts][Image1]

Vor dem Ausführen der Anwendung sollten Sie den Quellcode überprüfen. Um den Code zu anzuzeigen, wählen Sie in Visual Studio im Menü **Ansicht** die Option **Projektmappen-Explorer** aus. Doppelklicken Sie anschließend auf die Datei „Program.cs“.

Führen Sie nun die Beispielanwendung aus:

1. Wählen Sie in Visual Studio im Menü **Ansicht** die Option **Projektmappen-Explorer** aus. Öffnen Sie die Datei „App.config“, und kommentieren Sie die Verbindungszeichenfolge für den Azure-Speicheremulator aus:

   `<!--<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>-->`

2. Kommentieren Sie die Verbindungszeichenfolge für den Azure Storage-Dienst aus, und geben Sie den Speicherkontonamen und den Zugriffsschlüssel in der App.config-Datei an:

   `<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]"`

   Informationen zum Abrufen der Zugriffsschlüssel für Ihr Speicherkonto finden Sie unter [Verwalten von Speicherzugriffsschlüsseln](storage-create-storage-account.md#manage-your-storage-access-keys).
3. Klicken Sie nach Angabe des Speicherkontonamens und des Zugriffsschlüssels in der Datei „App.config“ im Menü **Datei** auf **Alle speichern**, um alle Projektdateien zu speichern.
4. Klicken Sie im Menü **Build** auf **Projektmappe erstellen**.
5. Drücken Sie im Menü **Debuggen** die Taste **F11**, um die Projektmappe Schritt für Schritt auszuführen, oder **F5**, um die Projektmappe auszuführen.

## <a name="run-your-first-azure-storage-application-locally-against-the-azure-storage-emulator"></a>Lokales Ausführen Ihrer ersten Azure Storage-Anwendung im Azure-Speicheremulator
Der [Azure-Speicheremulator](storage-use-emulator.md) bietet eine lokale Umgebung, die die Azure-Dienste für Blobs, Warteschlangen und Tabellenspeicher für Entwicklungszwecke emuliert. Sie können den Speicheremulator verwenden, um Ihre Anwendung lokal zu testen, ohne ein Azure-Abonnement oder ein Speicherkonto zu benötigen und ohne dass Kosten anfallen.

Um dies auszuprobieren, erstellen wir nun eine einfache Azure Storage-Anwendung mit einem der Azure-Schnellstart-Beispielprojekte in Visual Studio. Dieses Tutorial konzentriert sich auf Beispielprojekte für **Azure Blob Storage**, **Azure Table Storage** und **Azure Queue Storage**:

1. Starten Sie Visual Studio.
2. Klicken Sie im Menü **Datei** auf **Neues Projekt**.
3. Klicken Sie im Dialogfeld **Neues Projekt** auf **Installiert** > **Vorlagen** > **Visual C#** > **Cloud** > **Schnellstarts** > **Data Services**.
    a. Wählen Sie eine der folgenden Vorlagen aus: **Azure Storage: Blobs**, **Azure Storage: Dateien**, **Azure Storage: Warteschlangen** oder **Azure Storage: Tabellen**.
    b. Stellen Sie sicher, dass als Zielframework **.NET Framework 4.5** ausgewählt ist.
    c. Geben Sie einen Namen für das Projekt an, und erstellen Sie die neue Visual Studio-Projektmappe, wie hier zu sehen:

    ![Azure-Schnellstarts][Image1]

4. Wählen Sie in Visual Studio im Menü **Ansicht** die Option **Projektmappen-Explorer** aus. Öffnen Sie die Datei „App.config“, und kommentieren Sie die Verbindungszeichenfolge für das Azure-Speicherkonto aus, falls Sie bereits eins hinzugefügt haben. Heben Sie danach die Auskommentierung für die Verbindungszeichenfolge für den Azure-Speicheremulator auf.

   `<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>`

Vor dem Ausführen der Anwendung sollten Sie den Quellcode überprüfen. Um den Code zu anzuzeigen, wählen Sie in Visual Studio im Menü **Ansicht** die Option **Projektmappen-Explorer** aus. Doppelklicken Sie anschließend auf die Datei „Program.cs“.

Führen Sie nun die Beispielanwendung im Azure-Speicheremulator aus:

1. Klicken Sie auf die Schaltfläche **Start** , oder drücken Sie die Windows-Taste, suchen Sie nach *Microsoft Azure-Speicheremulator*, und starten Sie die Anwendung. Wenn der Emulator gestartet wird, werden in der Windows-Ansicht der aktiven Anwendungen ein Symbol und eine Benachrichtigung angezeigt.
2. Wählen Sie in Visual Studio im Menü **Projektmappe erstellen** on the **Projektmappe erstellen** aus.
3. Drücken Sie im Menü **Debuggen** die Taste **F11**, um die Projektmappe Schritt für Schritt auszuführen, oder **F5**, um die Projektmappe vollständig auszuführen.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Azure Storage finden Sie in den folgenden Ressourcen:

* [Einführung in Microsoft Azure Storage](storage-introduction.md)
* [Erste Schritte mit dem Speicher-Explorer (Vorschau)](../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Erste Schritte mit Azure Blob Storage mit .NET](storage-dotnet-how-to-use-blobs.md)
* [Erste Schritte mit Azure Table Storage mit .NET](storage-dotnet-how-to-use-tables.md)
* [Erste Schritte mit Azure Queue Storage mit .NET](storage-dotnet-how-to-use-queues.md)
* [Erste Schritte mit Azure File Storage unter Windows](storage-dotnet-how-to-use-files.md)
* [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](storage-use-azcopy.md)
* [Azure-Speicherdokumentation](https://azure.microsoft.com/documentation/services/storage/)
* [Microsoft Azure-Speicherclientbibliothek für .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [REST-API für Azure-Speicherdienste](https://msdn.microsoft.com/library/azure/dd179355.aspx)

[Image1]: ./media/storage-getting-started-guide/QuickStart.png



<!--HONumber=Feb17_HO3-->


