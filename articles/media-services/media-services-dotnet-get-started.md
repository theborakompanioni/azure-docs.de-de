---
title: Erste Schritte zum Bereitstellen von Inhalten nach Bedarf mithilfe von .NET | Microsoft Docs
description: "Dieses Lernprogramm führt Sie durch die Schritte zum Implementieren einer Anwendung zur Übermittlung von Inhalten nach Bedarf mit Azure Media Services mithilfe von .NET."
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 388b8928-9aa9-46b1-b60a-a918da75bd7b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 12/11/2016
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: 24d324a724792051eb6d86026da7b41ee9ff87b1
ms.openlocfilehash: 26720340d72c31016e51cc33589388780a2f4a8a


---

# <a name="get-started-with-delivering-content-on-demand-using-net-sdk"></a>Erste Schritte zum Bereitstellen von Inhalten nach Bedarf mit dem .NET SDK
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

> [!NOTE]
> Sie benötigen ein Azure-Konto, um dieses Lernprogramm auszuführen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F). 
> 
> 

## <a name="overview"></a>Übersicht
Dieses Lernprogramm führt Sie durch die Schritte zum Implementieren einer Anwendung zur Video-on-Demand (VoD)-Inhaltsübermittlung mithilfe des Azure Media Services (AMS) SDK für .NET.

Das Lernprogramm stellt den Media Services-Arbeitsfluss und die gängigsten Programmierobjekte und Aufgaben für die Media Services-Entwicklung vor. Nach Abschluss des Lernprogramms sind Sie in der Lage, eine einfache Mediendatei zu streamen oder progressiv herunterzuladen, die Sie hochgeladen, codiert und heruntergeladen haben.

## <a name="what-youll-learn"></a>Sie lernen Folgendes
Dieses Tutorial beschreibt die Durchführung der folgenden Aufgaben:

1. Erstellen eines Media Services-Kontos (mithilfe des Azure-Portals)
2. Konfigurieren von Streamingendpunkten (mithilfe des Azure-Portals)
3. Erstellen und Konfigurieren eines Visual Studio-Projekts
4. Herstellen einer Verbindung mit dem Media Services-Konto
5. Erstellen eines neuen Medienobjekts und Hochladen einer Videodatei
6. Codieren der Quelldatei in einen Satz von MP4-Dateien mit adaptiver Bitrate
7. Veröffentlichen des Medienobjekts und Abrufen von URLs für Streaming und progressiven Download
8. Testen der Funktionalität durch Wiedergabe von Inhalten.

## <a name="prerequisites"></a>Voraussetzungen
Zum Abschließen dieses Lernprogramms müssen folgende Voraussetzungen erfüllt sein:

* Sie benötigen ein Azure-Konto, um dieses Lernprogramm auszuführen. 
  
    Wenn Sie noch kein Konto haben, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F). Sie erhalten ein Guthaben, mit dem Sie andere kostenpflichtige Azure-Dienste ausprobieren können. Selbst, nachdem Sie dieses Guthaben aufgebraucht haben, können Sie das Konto behalten und kostenlose Azure-Dienste und -Features nutzen, z. B. das Web-Apps-Feature in Azure App Service.
* Betriebssysteme: Windows 8 oder höher, Windows 2008 R2, Windows 7.
* .NET Framework 4.0 oder höher
* Visual Studio 2010 SP1 (Professional, Premium, Ultimate oder Express) oder höhere Versionen.

## <a name="download-sample"></a>Beispiel herunterladen
Laden Sie [hier](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/)ein Beispiel herunter und führen Sie es aus.

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Erstellen eines Azure Media Services-Kontos mithilfe des Azure-Portals
In diesem Abschnitt erfahren Sie, wie Sie ein AMS-Konto erstellen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie auf **+Neu** > **Medien und CDN** > **Media Services**.
   
    ![Media Services – Erstellung](./media/media-services-portal-vod-get-started/media-services-new1.png)
3. Geben Sie unter **CREATE MEDIA SERVICES ACCOUNT** (MEDIA SERVICES-KONTO ERSTELLEN) die erforderlichen Werte ein.
   
    ![Media Services – Erstellung](./media/media-services-portal-vod-get-started/media-services-new3.png)
   
   1. Geben Sie unter **Kontoname**den Namen des neuen AMS-Kontos ein. Der Name eines Media Services-Kontos darf nur Kleinbuchstaben oder Ziffern ohne Leerzeichen enthalten und muss aus 3 bis 24 Zeichen bestehen.
   2. Wählen Sie unter „Abonnement“ das Azure-Abonnement aus, auf das Sie Zugriff haben.
   3. Wählen Sie unter **Ressourcengruppe**die neue oder vorhandene Ressource aus.  Eine Ressourcengruppe ist eine Sammlung von Ressourcen mit gleichem Lebenszyklus, gleichen Berechtigungen und gleichen Richtlinien. [Hier](../azure-resource-manager/resource-group-overview.md#resource-groups) erhalten Sie weitere Informationen.
   4. Wählen Sie unter **Standort** die geografische Region aus, in der die Medien- und Metadaten-Datensätze für Ihr Media Services-Konto gespeichert werden. Dieser Bereich wird zum Verarbeiten und Streamen Ihrer Medien verwendet. Im Dropdownlistenfeld werden nur die verfügbaren Media Services-Regionen angezeigt. 
   5. Wählen Sie unter **Speicherkonto**ein Speicherkonto aus, das als Blobspeicher für die Medieninhalte aus Ihrem Media Services-Konto dienen soll. Sie können ein vorhandenes Speicherkonto in der gleichen geografischen Region auswählen, in der sich auch Ihr Media Services-Konto befindet, oder ein Speicherkonto erstellen. Ein neues Speicherkonto wird in derselben Region erstellt. Für Namen von Speicherkonten gelten die gleichen Regeln wie für Namen von Media Services-Konten.
      
       Weitere Informationen zum Speicher finden Sie [hier](../storage/storage-introduction.md).
   6. Wählen Sie **An Dashboard anheften** aus, um den Status der Kontobereitstellung anzuzeigen.
4. Klicken Sie unten im Formular auf **Erstellen** .
   
    Wenn das Konto erfolgreich erstellt wurde, ändert sich der Status in **Wird ausgeführt**. 
   
    ![Media Services – Einstellungen](./media/media-services-portal-vod-get-started/media-services-settings.png)
   
    Verwenden Sie zum Verwalten Ihres AMS-Kontos (Hochladen von Videos, Codieren von Ressourcen, Überwachen des Auftragsstatus und Ähnliches) das Fenster **Einstellungen** .

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Konfigurieren von Streamingendpunkten mithilfe des Azure-Portals
Wenn Sie mit Azure Media Services arbeiten, besteht eines der häufigsten Szenarien darin, Videos per Adaptive Bitrate Streaming an Ihre Clients zu übermitteln. Von Media Services werden die folgenden Technologien mit Adaptive Bitrate Streaming unterstützt: HTTP Live Streaming (HLS), Smooth Streaming und MPEG DASH.

Media Services bietet dynamische Paketerstellung für die Just-in-Time-Übermittlung von Daten vom Typ „MP4-codierte Inhalte mit adaptiver Bitrate“ in Streamingformaten, die von Media Services unterstützt werden (MPEG DASH, HLS, Smooth Streaming), ohne dass Sie jeweils vorab verpackte Versionen dieser Streamingformate speichern müssen.

Um die dynamische Paketerstellung nutzen zu können, müssen Sie folgende Schritte ausführen:

* Codieren Ihrer Zwischendatei (Quelle) in einen Satz von MP4-Dateien mit adaptiver Bitrate (die Codierungsschritte werden weiter unten in diesem Tutorial beschrieben)  
* Erstellen von mindestens einer Streamingeinheit für den *Streamingendpunkt*, von dem aus Sie die Bereitstellung Ihrer Inhalte planen In den folgenden Schritten wird gezeigt, wie Sie die Anzahl von Streamingeinheiten ändern.

Mit der dynamischen Paketerstellung müssen Sie die Dateien nur in einem einzelnen Speicherformat speichern und bezahlen. Media Services erstellt und verarbeitet die entsprechende Antwort basierend auf Anforderungen von einem Client.

Gehen Sie wie folgt vor, um die Anzahl von Einheiten zu erstellen und zu ändern, die für das Streaming reserviert sind:

1. Klicken Sie im Fenster **Einstellungen** auf **Streamingendpunkte**. 
2. Klicken Sie auf den standardmäßigen Streamingendpunkt. 
   
    Das Fenster **DEFAULT STREAMING ENDPOINT DETAILS** (DETAILS ZUM STANDARD-STREAMINGENDPUNKT) wird angezeigt.
3. Die Anzahl von Streamingeinheiten kann mithilfe des Schiebereglers **Streamingeinheiten** angegeben werden.
   
    ![Streamingeinheiten](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)
4. Klicken Sie auf die Schaltfläche **Speichern** , um die Änderungen zu speichern.
   
   > [!NOTE]
   > Es kann bis zu 20 Minuten dauern, bis die Zuordnung neuer Einheiten abgeschlossen ist.
   > 
   > 

## <a name="create-and-configure-a-visual-studio-project"></a>Erstellen und Konfigurieren eines Visual Studio-Projekts

1. Erstellen Sie eine neue C#-Konsolenanwendung in Visual Studio 2013, Visual Studio 2012 oder Visual Studio 2010 SP1. Geben Sie **Name**, **Speicherort** und **Projektmappenname** ein, und klicken Sie auf **OK**.
2. Verwenden Sie das NuGet-Paket [windowsazure.mediaservices.extensions](https://www.nuget.org/packages/windowsazure.mediaservices.extensions), um die **Azure Media Services .NET SDK-Erweiterungen**zu installieren.  Media Services SDK-Erweiterungen für .NET ist ein Satz von Erweiterungsmethoden und Hilfsfunktionen, die Ihren Code vereinfachen und eine einfachere Entwicklung mit Media Services ermöglichen. Durch Installieren dieses Pakets werden auch das **Media Services .NET SDK** installiert und alle anderen erforderlichen Abhängigkeiten hinzugefügt.

    Gehen Sie wie folgt vor, um mit NuGet Verweise hinzuzufügen: Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie **NuGet-Pakete verwalten**. Suchen Sie dann nach **windowsazure.mediaservices.extensions**, und klicken Sie auf **Installieren**.

3. Fügen Sie einen Verweis auf die Assembly "System.Configuration" hinzu. Diese Assembly enthält die **System.Configuration.ConfigurationManager**-Klasse, die zum Zugriff auf die Konfigurationsdateien (z.B. „App.config“) verwendet wird.

    Gehen Sie wie folgt vor, um einen Verweis hinzuzufügen: Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, wählen Sie **Hinzufügen** > **Verweis...**, und geben Sie im Suchfeld den Begriff „Konfiguration“ ein. 

4. Öffnen Sie die Datei "App.config" (fügen Sie die Datei dem Projekt hinzu, wenn sie nicht standardmäßig hinzugefügt wurde), und fügen Sie einen *appSettings* -Abschnitt in der Datei hinzu. Legen Sie die Werte für Ihren Azure Media Services-Kontonamen und Schlüssel fest, wie im folgenden Beispiel gezeigt. Um den Kontonamen und wichtige Informationen zu erhalten, wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihr AMS-Konto aus. Wählen Sie dann **Einstellungen** > **Schlüssel** aus. Im Fenster „Schlüssel verwalten“ werden der Kontoname sowie der Primär- und Sekundärschlüssel angezeigt. Kopieren Sie die Werte für den Kontonamen und den Primärschlüssel.
   
        <configuration>
        ...
          <appSettings>
            <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
            <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
          </appSettings>
   
        </configuration>
5. Überschreiben Sie die existierenden **using** -Anweisungen am Anfang der Datei "Program.cs" durch den folgenden Code.
   
        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
6. Erstellen Sie einen neuen Ordner unter dem Verzeichnis Projekte, und kopieren Sie eine MP4 oder WMV-Datei, die Sie codieren und streamen oder progressiv herunterladen möchten. In diesem Beispiel wird der Pfad "C:\VideoFiles" verwendet.

## <a name="connect-to-the-media-services-account"></a>Herstellen einer Verbindung mit dem Media Services-Konto

Wenn Sie Media Services mit .NET verwenden, müssen Sie die **CloudMediaContext**-Klasse für die meisten Media Services-Programmieraufgaben verwenden, z.B. für das Herstellen einer Verbindung mit dem Media Services-Konto, das Erstellen, Aktualisieren, Zugreifen auf und Löschen der folgenden Objekte: Medienobjekte, Objektdateien, Aufträge, Zugriffsrichtlinien, Locators usw.

Überschreiben Sie die Standardklasse des Programms durch den folgenden Code. Im Code wird veranschaulicht, wie die Werte aus der Datei "App.config" gelesen werden und das **CloudMediaContext** -Objekt erstellt wird, um eine Verbindung zu Media Services herzustellen. Weitere Informationen zum Herstellen einer Verbindung mit Media Services finden Sie unter [Herstellen einer Verbindung zu Media Services mit dem Media Services SDK für .NET](http://msdn.microsoft.com/library/azure/jj129571.aspx).


Die **Main** -Funktion ruft Methoden auf, die weiter unten in diesem Abschnitt definiert werden.

> [!NOTE]
> Sie erhalten so lange Kompilierungsfehler, bis Sie Definitionen für alle Funktionen hinzufügen.

    class Program
    {
        // Read values from the App.config file.
        private static readonly string _mediaServicesAccountName =
            ConfigurationManager.AppSettings["MediaServicesAccountName"];
        private static readonly string _mediaServicesAccountKey =
            ConfigurationManager.AppSettings["MediaServicesAccountKey"];

        // Field for service context.
        private static CloudMediaContext _context = null;
        private static MediaServicesCredentials _cachedCredentials = null;

        static void Main(string[] args)
        {
            try
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the chached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Add calls to methods defined in this section.

                IAsset inputAsset =
                    UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

                IAsset encodedAsset =
                    EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

                PublishAssetGetURLs(encodedAsset);
            }
            catch (Exception exception)
            {
                // Parse the XML error message in the Media Services response and create a new
                // exception with its content.
                exception = MediaServicesExceptionParser.Parse(exception);

                Console.Error.WriteLine(exception.Message);
            }
            finally
            {
                Console.ReadLine();
            }
        }
    }

## <a name="create-a-new-asset-and-upload-a-video-file"></a>Erstellen eines neuen Medienobjekts und Hochladen einer Videodatei

In Media Services laden Sie Ihre digitalen Dateien in ein Medienobjekt hoch oder erfassen sie auf diese Weise. Die **Asset**-Entität kann Videos, Audiodateien, Bilder, Miniaturansichtssammlungen, Texttitel und Untertiteldateien (und die Metadaten zu diesen Dateien) enthalten.  Nachdem die Dateien hochgeladen wurden, werden Ihre Inhalte zur weiteren Verarbeitung und zum Streaming sicher in der Cloud gespeichert. Die Dateien im Medienobjekt heißen **Medienobjektdateien**.

Die unten definierte **UploadFile**-Methode ruft **CreateFromFile** auf (in den .NET SDK-Erweiterungen definiert). **CreateFromFile** erstellt ein neues Medienobjekt, in das die angegebene Quelldatei hochgeladen wird.

Die **CreateFromFile**-Methode übernimmt **AssetCreationOptions**, sodass Sie eine der folgenden Erstellungsoptionen für Medienobjekte angeben können:

* **None:** Es wird keine Verschlüsselung verwendet. Dies ist der Standardwert. Beachten Sie, dass bei Verwendung dieser Option Ihre Inhalte während der Übertragung oder des Verbleibs im Speicher nicht geschützt sind.
  Wenn Sie planen, eine MP4-Datei über progressives Herunterladen zu übermitteln, verwenden Sie diese Option.
* **StorageEncrypted:** Verwenden Sie diese Option, um Ihre unverschlüsselten Inhalte lokal mithilfe der AES-256-Bit-Verschlüsselung (Advanced Encryption Standard) zu verschlüsseln. Anschließend werden sie in Azure Storage hochgeladen, wo sie verschlüsselt im Ruhezustand gespeichert werden. Medienobjekte, die durch Storage Encryption geschützt sind, werden automatisch entschlüsselt, vor der Codierung in einem verschlüsselten Dateisystem platziert und optional vor dem Hochladen als neues Ausgabemedienobjekt erneut verschlüsselt. Der primäre Anwendungsfall für Storage Encryption ist, wenn Sie Ihre qualitativ hochwertigen Eingabemediendateien mit starker Verschlüsselung beim Speichern im Ruhezustand auf dem Datenträger sichern möchten.
* **CommonEncryptionProtected:** Verwenden Sie diese Option, wenn Sie Inhalte hochladen, die bereits verschlüsselt wurden und durch allgemeine Verschlüsselung oder PlayReady-DRM geschützt sind (z.B. mit PlayReady-DRM geschütztem Smooth Streaming).
* **EnvelopeEncryptionProtected** : Verwenden Sie diese Option, wenn Sie mit AES verschlüsseltes HLS hochladen. Beachten Sie, dass die Dateien durch Transform Manager codiert und verschlüsselt sein müssen.

Mit der **CreateFromFile** -Methode können Sie auch einen Rückruf angeben, um den Fortschritt des Dateiuploads zu melden.

Im folgenden Beispiel wird **Kein** für die Medienobjektoptionen angegeben.

Fügen Sie die folgende Methode zur Program-Klasse hinzu.

    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }


## <a name="encode-the-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a>Codieren der Quelldatei in einen Satz von MP4-Dateien mit adaptiver Bitrate
Nach dem Erfassen der Medienobjekte in Media Services können die Medien u. a. codiert, transcodiert/multiplexiert und mit einem Wasserzeichen versehen werden, bevor sie an die Clients übermittelt werden. Diese Aktivitäten werden geplant und für mehrere Hintergrundrolleninstanzen ausgeführt, um hohe Leistung und Verfügbarkeit zu gewährleisten. Diese Aktivitäten werden als Aufträge bezeichnet, und jeder Auftrag besteht aus atomaren Tasks, welche die eigentliche Arbeit für die Medienobjektdatei leisten.

Wie bereits erwähnt, besteht beim Arbeiten mit Azure Media Services eines der häufigsten Szenarios darin, Adaptive Bitrate Streaming an Ihre Clients zu übermitteln. Media Services kann eine Reihe von MP4-Dateien mit adaptiver Bitrate dynamisch in eines der folgenden Formate verpacken: HTTP Live Streaming (HLS), Smooth Streaming und MPEG DASH.

Um die dynamische Paketerstellung nutzen zu können, müssen Sie folgende Schritte ausführen:

* Codieren oder Transcodieren Ihrer Zwischendatei (Quelldatei) in einen Satz von MP4-Dateien oder Smooth Streaming-Dateien mit adaptiver Bitrate  
* Abrufen von mindestens einer Streamingeinheit für den Streamingendpunkt, von dem aus Sie die Bereitstellung Ihrer Inhalte planen

Der folgende Code zeigt, wie Sie einen Codierungsauftrag senden. Der Auftrag enthält eine Aufgabe zum Transcodieren der Zwischendatei in eine Reihe von MP4-Dateien mit adaptiver Bitrate mithilfe von **Media Encoder Standard**. Der Code sendet den Auftrag und wartet, bis er abgeschlossen ist.

Wenn der Auftrag abgeschlossen ist, sind Sie in der Lage, das Medienobjekt zu streamen oder MP4-Dateien, die als Ergebnis der Transcodierung erstellt wurden, progressiv herunterzuladen.
Beachten Sie, dass Sie keine Streamingeinheiten abrufen müssen, um MP4-Dateien progressiv herunterzuladen.

Fügen Sie die folgende Methode zur Program-Klasse hinzu.

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {

        // Prepare a job with a single task to transcode the specified asset
        // into a multi-bitrate asset.

        IJob job = _context.Jobs.CreateWithSingleTask(
            "Media Encoder Standard",
            "H264 Multiple Bitrate 720p",
            asset,
            "Adaptive Bitrate MP4",
            options);

        Console.WriteLine("Submitting transcoding job...");


        // Submit the job and wait until it is completed.
        job.Submit();

        job = job.StartExecutionProgressTask(
            j =>
            {
                Console.WriteLine("Job state: {0}", j.State);
                Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
            },
            CancellationToken.None).Result;

        Console.WriteLine("Transcoding job finished.");

        IAsset outputAsset = job.OutputMediaAssets[0];

        return outputAsset;
    }

## <a name="publish-the-asset-and-get-urls-for-streaming-and-progressive-download"></a>Veröffentlichen des Medienobjekts und Abrufen von URLs für Streaming und progressiven Download

Um ein Medienobjekt zu streamen oder herunterzuladen, müssen Sie es zunächst durch Erstellen eines Locators "veröffentlichen". Locators ermöglichen den Zugriff auf Dateien im Medienobjekt. Media Services unterstützt zwei Locatortypen: OnDemandOrigin-Locators zum Streamen von Medien (z.B. MPEG DASH, HLS oder Smooth Streaming) und SAS-Locators (Shared Access Signature) zum Herunterladen von Mediendateien. (Weitere Informationen zu SAS-Locators finden Sie in [diesem Blog](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/).)

Nachdem Sie die Locator erstellt haben, können Sie die URLs erstellen, mit denen Sie die Dateien streamen oder herunterladen möchten.

Eine URL für Smooth Streaming hat das folgende Format:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Eine Streaming-URL für HLS hat das folgende Format:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Eine Streaming-URL für MPEG DASH hat das folgende Format:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

Eine SAS-URL zum Herunterladen von Dateien hat das folgende Format:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Media Services .NET SDK-Erweiterungen bieten praktische Hilfsmethoden, die formatierte URLs für das veröffentlichte Medienobjekt zurückgeben.

Der folgende Code verwendet .NET SDK-Erweiterungen, um Locators zu erstellen sowie Streaming-URLs und URLs für progressive Downloads abzurufen. Der Code zeigt auch das Herunterladen von Dateien in einen lokalen Ordner.

Fügen Sie die folgende Methode zur Program-Klasse hinzu.

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish the output asset by creating an Origin locator for adaptive streaming,
        // and a SAS locator for progressive download.

        _context.Locators.Create(
            LocatorType.OnDemandOrigin,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));

        _context.Locators.Create(
            LocatorType.Sas,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));


        IEnumerable<IAssetFile> mp4AssetFiles = asset
                .AssetFiles
                .ToList()
                .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Get the Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and the Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get the URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  the streaming URLs.
        Console.WriteLine("Use the following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display the URLs for progressive download.
        Console.WriteLine("Use the following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download the output asset to a local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files to a local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

## <a name="test-by-playing-your-content"></a>Testen der Funktionalität durch Wiedergabe von Inhalten
Nachdem Sie das im vorherigen Abschnitt definierte Programm ausgeführt haben, werden ähnliche URLs wie die folgenden im Konsolenfenster angezeigt.

Adaptive Streaming-URLs:

Smooth Streaming

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG DASH

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

URLs für progressiven Download (Audio und Video).

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


Verwenden Sie zum Streamen von Videos [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

Fügen Sie zum Testen des progressiven Downloads eine URL in einen Browser ein (z. B. Internet Explorer, Chrome oder Safari).

## <a name="next-steps-media-services-learning-paths"></a>Nächste Schritte: Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="looking-for-something-else"></a>Suchen Sie etwas anderes?
Wenn dieses Thema nicht die erwarteten Informationen enthält, Informationen fehlen oder auf andere Weise Ihre Erwartungen nicht erfüllt wurden, senden Sie uns bitte über den Disqus-Thread unten Ihr Feedback.

<!-- Anchors. -->


<!-- URLs. -->
[Web Platform Installer]: http://go.microsoft.com/fwlink/?linkid=255386
[Portal]: http://manage.windowsazure.com/



<!--HONumber=Dec16_HO2-->


