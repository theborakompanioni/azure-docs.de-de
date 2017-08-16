---
title: "Verwenden von .NET SDK für Microsoft Azure StorSimple Data Manager-Aufträge | Microsoft Docs"
description: "Erfahren Sie, wie Sie .NET SDK zum Ausführen von StorSimple Data Manager-Aufträgen verwenden (private Vorschau)"
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/22/2016
ms.author: vidarmsft
ms.translationtype: HT
ms.sourcegitcommit: caaf10d385c8df8f09a076d0a392ca0d5df64ed2
ms.openlocfilehash: 44d243a034b20b99faf284c8615e470bc6f9d020
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---

# <a name="use-the-net-sdk-to-initiate-data-transformation-private-preview"></a>Verwenden des .NET SDK zum Initiieren einer Datentransformation (private Vorschau)

## <a name="overview"></a>Übersicht

In diesem Artikel wird erläutert, wie Sie die Datentransformationsfunktion innerhalb des StorSimple Data Manager-Diensts verwenden, um StorSimple-Gerätedaten zu transformieren. Die transformierten Daten können dann von anderen Azure-Diensten in der Cloud verwendet werden. In diesem Artikel finden Sie auch eine exemplarische Vorgehensweise zum Erstellen einer Beispiel-.NET-Konsolenanwendung zum initiieren eines Datentransformationsauftrags und zum Nachverfolgen der Fertigstellung.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen stellen Sie sicher, dass Sie über Folgendes verfügen:
*   Ein System, auf dem Visual Studio 2012, 2013, 2015 oder 2017 installiert ist.
*   Azure PowerShell ist installiert. [Azure PowerShell herunterladen](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
*   Konfigurationseinstellungen für das Initiieren des Datentransformationsauftrags (in diesem Dokument finden Sie Anweisungen, wie Sie diese Einstellungen erhalten).
*   Eine Auftragsdefinition, die korrekt in einer Ressource für Hybriddaten innerhalb einer Ressourcengruppe konfiguriert wurde.
*   Alle erforderlichen DLLS. Laden Sie diese DLLS aus dem [GitHub-Repository](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) herunter.
*   `Get-ConfigurationParams.ps1` [-Skript](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Data_Manager_Job_Run/Get-ConfigurationParams.ps1) aus dem GitHub-Repository.

## <a name="step-by-step"></a>Schrittweise Anleitung

Führen Sie die folgenden Schritte aus, um .NET für das Starten eines Datentransformationsauftrags zu verwenden.

1. Führen Sie die folgenden Schritte aus, um die Konfigurationsparameter abzurufen:
    1. Laden Sie `Get-ConfigurationParams.ps1` aus dem GitHub-Repository-Skript in `C:\DataTransformation` herunter.
    1. Führen Sie das `Get-ConfigurationParams.ps1`-Skript aus dem GitHub-Repository heraus aus. Geben Sie folgenden Befehl ein:

        ```
        C:\DataTransformation\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```
        Sie können beliebige Werte für ActiveDirectoryKey und AppName übergeben.


2. Dieses Skript gibt die folgenden Werte aus:
    * Client-ID
    * Mandanten-ID
    * Active Directory-Schlüssel (identisch mit der Eingabe für „ActiveDirectoryKey“)
    * Abonnement-ID

3. Erstellen Sie mithilfe von Visual Studio 2012, 2013 oder 2015 eine C# .NET-Konsolenanwendung.

    1. Starten Sie **Visual Studio 2012/2013/2015**.
    1. Klicken Sie auf **Datei**, zeigen Sie auf **Neu**, und klicken Sie auf **Projekt**.
    2. Erweitern Sie **Vorlagen**, und wählen Sie **Visual C#** aus.
    3. Wählen Sie in der Liste der Projekttypen auf der rechten Seite **Konsolenanwendung** aus.
    4. Geben Sie bei **Name** **DataTransformationApp** ein.
    5. Wählen Sie **C:\DataTransformation** als **Speicherort** aus.
    6. Klicken Sie auf **OK** , um das Projekt zu erstellen.

4.  Fügen Sie nun alle DLLS, die sich im Ordner [dlls](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) befinden, als **Verweise** zum Projekt hinzu, das Sie erstellt haben. Führen Sie die folgenden Schritte aus, um die DLL-Dateien herunterzuladen:

    1. Wechseln Sie in Visual Studio zu **Ansicht > Projektmappen-Explorer**.
    1. Klicken Sie auf den Pfeil auf der linken Seite des Data Transformation-App-Projekts. Klicken Sie auf **Verweise**, und klicken Sie dann mit der rechten Maustaste auf **Verweis hinzufügen**.
    2. Suchen Sie den Speicherort des Paketordners, wählen Sie alle DLLs aus und klicken Sie auf **Hinzufügen**. Klicken Sie anschließend auf **OK**.

5. Fügen Sie die folgenden **using** -Anweisungen zur Quelldatei (Program.cs) im Projekt hinzu.

    ```
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using Microsoft.Azure.Management.HybridData.Models;
    using Microsoft.Internal.Dms.DmsWebJob;
    using Microsoft.Internal.Dms.DmsWebJob.Contracts;
    ```


6. Der folgende Code initialisiert die Instanz des Datentransformationsauftrags. Fügen Sie ihn in **Main-Methode** hinzu. Ersetzen Sie die Werte der Konfigurationsparameter, die Sie zuvor erhalten haben. Geben Sie die Werte für den **Ressourcengruppennamen** und **Hybrid Data Resource name (Name der Hybriddaten-Ressource)** ein. Der **Ressourcengruppenname** ist der, der die Hybriddaten-Ressource hostet, in der die Auftragsdefinition konfiguriert wurde.

    ```
    // Setup the configuration parameters.
    var configParams = new ConfigurationParams
    {
        ClientId = "client-id",
        TenantId = "tenant-id",
        ActiveDirectoryKey = "active-directory-key",
        SubscriptionId = "subscription-id",
        ResourceGroupName = "resource-group-name",
        ResourceName = "resource-name"
    };

    // Initialize the Data Transformation Job instance.
    DataTransformationJob dataTransformationJob = new DataTransformationJob(configParams);

    ```

7. Geben Sie die Parameter an, mit denen die Auftragsdefinition ausgeführt werden muss.

    ```
    string jobDefinitionName = "job-definition-name";

    DataTransformationInput dataTransformationInput = dataTransformationJob.GetJobDefinitionParameters(jobDefinitionName);

    ```

    (ODER)

    Wenn Sie die Parameter der Auftragsdefinition während der Laufzeit ändern möchten, fügen Sie den folgenden Code hinzu:

    ```
    string jobDefinitionName = "job-definition-name";
    // Must start with a '\'
    var rootDirectories = new List<string> {@"\root"};

    // Name of the volume on the StorSimple device.
    var volumeNames = new List<string> {"volume-name"};

    var dataTransformationInput = new DataTransformationInput
    {
        // If you require the latest existing backup to be picked else use TakeNow to trigger a new backup.
        BackupChoice = BackupChoice.UseExistingLatest.ToString(),
        // Name of the StorSimple device.
        DeviceName = "device-name",
        // Name of the container in Azure storage where the files will be placed after execution.
        ContainerName = "container-name",
        // File name filter (search pattern) to be applied on files under the root directory. * - Match all files.
        FileNameFilter = "*",
        // List of root directories.
        RootDirectories = rootDirectories,
        // Name of the volume on StorSimple device on which the relevant data is present. 
        VolumeNames = volumeNames
    };
    
    ```

8. Fügen Sie nach der Initialisierung den folgenden Code ein, um einen Datentransformationsauftrag für die Auftragsdefinition auszulösen. Geben Sie den passenden **Job Definition Name (Name der Auftragsdefinition)** ein.

    ```
    // Trigger a job, retrieve the jobId and the retry interval for polling.
    int retryAfter;
    string jobId = dataTransformationJob.RunJobAsync(jobDefinitionName, 
    dataTransformationInput, out retryAfter);

    ```

9. Durch diesen Auftrag werden die übereinstimmenden Dateien, die sich im Stammverzeichnis befinden, auf das StorSimple-Volume in den angegebenen Container hochgeladen. Wenn eine Datei hochgeladen wird, wird eine Nachricht in der Warteschlange (im gleichen Speicherkonto wie der Container) mit dem gleichen Namen wie die Auftragsdefinition abgelegt. Sie können diese Nachricht verwenden, um eine weitere Verarbeitung der Datei auszulösen.

10. Fügen Sie den folgenden Code hinzu, um die Fertigstellung des Auftrags zu überwachen, sobald der Auftrag ausgelöst wurde.

    ```
    Job jobDetails = null;

    // Poll the job.
    do
    {
        jobDetails = dataTransformationJob.GetJob(jobDefinitionName, jobId);

        // Wait before polling for the status again.
        Thread.Sleep(TimeSpan.FromSeconds(retryAfter));

    } while (jobDetails.Status == JobStatus.InProgress);

    // Completion status of the job.
    Console.WriteLine("JobStatus: {0}", jobDetails.Status);
    
    // To hold the console before exiting.
    Console.Read();

    ```


## <a name="next-steps"></a>Nächste Schritte

[Use StorSimple Data Manager UI to transform your data (Verwenden der StorSimple Data Manager-UI zum Transformieren von Daten) ](storsimple-data-manager-ui.md).

