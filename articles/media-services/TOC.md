# [Übersicht](media-services-overview.md)
## [Szenarien und Verfügbarkeit](scenarios-and-availability.md)
## [Konzepte](media-services-concepts.md)

# Erste Schritte
## [Erstellen und Verwalten eines Kontos](media-services-portal-create-account.md)
## [Einrichten Ihrer Entwicklungsumgebung](media-services-set-up-computer.md)
### [.NET](media-services-dotnet-how-to-use.md)
### [REST](media-services-rest-how-to-use.md)  
## [Verwenden der AAD-Authentifizierung für den API-Zugriff](media-services-use-aad-auth-to-access-ams-api.md)
### [Verwenden des Portals zum Verwalten der AAD-Authentifizierung](media-services-portal-get-started-with-aad.md)
### [API-Zugriff mit .NET](media-services-dotnet-get-started-with-aad.md)
### [API-Zugriff mit REST](media-services-rest-connect-with-aad.md)
### [Verwenden der CLI zum Erstellen und Konfigurieren der AAD-Anwendung](media-services-cli-create-and-configure-aad-app.md)
### [Verwenden von PowerShell zum Erstellen und Konfigurieren der AAD-Anwendung](media-services-powershell-create-and-configure-aad-app.md)

## Übertragen von Video on Demand
### [Portal](media-services-portal-vod-get-started.md)
### [.NET SDK](media-services-dotnet-get-started.md)
### [Java](media-services-java-how-to-use.md)
### [REST](media-services-rest-get-started.md)
## Ausführen von Livestreaming
### [Portal](media-services-portal-live-passthrough-get-started.md)
### [.NET](media-services-dotnet-live-encode-with-onpremises-encoders.md)

# Anleitung
## Verwalten
### Konten
#### [PowerShell](media-services-manage-with-powershell.md)
#### [REST](/rest/api/media/mediaservice)
### Entitäten
#### [.NET](media-services-dotnet-manage-entities.md)
#### [REST](media-services-rest-manage-entities.md)
### [Streamingendpunkte](media-services-streaming-endpoints-overview.md)
#### [Portal](media-services-portal-manage-streaming-endpoints.md)
#### [.NET](media-services-dotnet-manage-streaming-endpoints.md)
### Speicher
#### [Aktualisieren von Media Services nach dem Austausch der Speicherzugriffsschlüssel](media-services-roll-storage-access-keys.md)
#### [Speicherkontenübergreifendes Verwalten von Assets](meda-services-managing-multiple-storage-accounts.md)
### [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md)

## Hochladen von Inhalten
### Hochladen von Dateien in ein Konto
#### [Portal](media-services-portal-upload-files.md)
#### [.NET](media-services-dotnet-upload-files.md)
#### [REST](media-services-rest-upload-files.md)
### [Hochladen großer Dateien mit Aspera](media-services-upload-files-with-aspera.md)
### [Hochladen von Dateien mit StorSimple](media-services-upload-files-from-storsimple.md)
### [Kopieren vorhandener Blobs](media-services-copying-existing-blob.md)

## [Codieren von Inhalten](media-services-encode-asset.md)
### [Vergleich der Encoder](media-services-compare-encoders.md)
### [Verwalten von Geschwindigkeit und Parallelität der Codierung](media-services-manage-encoding-speed.md)
### Media Encoder Standard (MES)
#### [Media Encoder Standard-Formate und -Codecs](media-services-media-encoder-standard-formats.md)
#### [Verwenden von MES zum automatischen Generieren einer Bitrate](media-services-autogen-bitrate-ladder-with-mes.md)
#### Codieren mit Media Encoder Standard
##### [Portal](media-services-portal-encode.md)
##### [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
##### [REST](media-services-rest-encode-asset.md)
#### [Erweiterte Codierungsfunktionen mit MES](media-services-advanced-encoding-with-mes.md)
##### [Anpassen von Media Encoder Standard-Voreinstellungen](media-services-custom-mes-presets-with-dotnet.md)
##### [Generieren von Miniaturansichten mithilfe von Media Encoder Standard mit .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
##### [Zuschneiden von Videos mit Media Encoder Standard](media-services-crop-video.md)
#### MES-Schemas
##### [Media Encoder Standard-Schema](media-services-mes-schema.md)
##### [Eingeben von Metadaten](media-services-input-metadata-schema.md)
##### [Ausgeben von Metadaten](media-services-output-metadata-schema.md)
#### [MES-Voreinstellungen](media-services-mes-presets-overview.md) 
##### [H264 Multiple Bitrate 1080p Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-1080p-Audio-5.1.md)
##### [H264 Multiple Bitrate 1080p](media-services-mes-preset-H264-Multiple-Bitrate-1080p.md)
##### [H264 Multiple Bitrate 16x9 SD Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD-Audio-5.1.md)
##### [H264 Multiple Bitrate 16x9 SD](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD.md)
##### [H264 Multiple Bitrate 16x9 für iOS](media-services-mes-preset-H264-Multiple-Bitrate-16x9-for-iOS.md)
##### [H264 Multiple Bitrate 4K Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4K-Audio-5.1.md)
##### [H264 Multiple Bitrate 4K](media-services-mes-preset-H264-Multiple-Bitrate-4K.md)
##### [H264 Multiple Bitrate 4x3 SD Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD-Audio-5.1.md)
##### [H264 Multiple Bitrate 4x3 SD](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD.md)
##### [H264 Multiple Bitrate 4x3 für iOS](media-services-mes-preset-H264-Multiple-Bitrate-4x3-for-iOS.md)
##### [H264 Multiple Bitrate 720p Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-720p-Audio-5.1.md)
##### [H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md)
##### [H264 Single Bitrate 1080p Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-1080p-Audio-5.1.md)
##### [H264 Single Bitrate 1080p](media-services-mes-preset-H264-Single-Bitrate-1080p.md)
##### [H264 Single Bitrate 16x9 SD Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-16x9-SD-Audio-5.1.md)
##### [H264 Single Bitrate 16x9 SD](media-services-mes-preset-H264-Single-Bitrate-16x9-SD.md)
##### [H264 Single Bitrate 4K Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-4K-Audio-5.1.md)
##### [H264 Single Bitrate 4K](media-services-mes-preset-H264-Single-Bitrate-4K.md)
##### [H264 Single Bitrate 4x3 SD Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-4x3-SD-Audio-5.1.md)
##### [H264 Single Bitrate 4x3 SD](media-services-mes-preset-H264-Single-Bitrate-4x3-SD.md)
##### [H264 Single Bitrate 720p Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-720p-Audio-5.1.md)
##### [H264 Single Bitrate 720p](media-services-mes-preset-H264-Single-Bitrate-720p.md)
##### [H264 Single Bitrate 720p für Android](media-services-mes-preset-H264-Single-Bitrate-720p-for-Android.md)
##### [H264 Single Bitrate High Quality SD für Android](media-services-mes-preset-H264-Single-Bitrate-High-Quality-SD-for-Android.md)
##### [H264 Single Bitrate Low Quality SD für Android](media-services-mes-preset-H264-Single-Bitrate-Low-Quality-SD-for-Android.md)
### Media Encoder Premium Workflow
#### [Media Encoder Premium Workflow-Formate und -Codecs](media-services-premium-workflow-encoder-formats.md)
#### Codierung mit dem Medienencoder-Premium-Workflow
##### [Media Encoder Premium-Workflow](media-services-encode-with-premium-workflow.md)
##### [Tutorials für den Media Encoder Premium-Workflow](media-services-media-encoder-premium-workflow-tutorials.md)
##### [Erstellen von erweiterten Codierungsworkflows mit Workflow-Designer](media-services-workflow-designer.md)
##### [Premium-Workflow mit mehreren Eingaben](media-services-media-encoder-premium-workflow-multiplefilesinput.md)
### [Erstellen einer Aufgabe, die fMP4-Abschnitte generiert](media-services-generate-fmp4-chunks.md)
### Medienprozessoren
#### [.NET](media-services-get-media-processor.md)
#### [REST](media-services-rest-get-media-processor.md)
### [Fehlercodes](media-services-encoding-error-codes.md)
### Veraltet
#### [Statische Paketerstellung und Verschlüsselung](media-services-static-packaging.md)

## [Live streamen](media-services-manage-channels-overview.md)
### [Lokale Encoder](media-services-live-streaming-with-onprem-encoders.md)
#### [Portal](media-services-portal-live-passthrough-get-started.md)
#### [.NET](media-services-dotnet-live-encode-with-onpremises-encoders.md)
#### [REST](https://docs.microsoft.com/rest/api/media/operations/channel)
### [Livestreaming mit Cloudencoder](media-services-manage-live-encoder-enabled-channels.md)
#### [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
#### [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
### [Konfigurieren von lokalen Encodern für die Verwendung mit einem Cloudencoder](media-services-live-encoders-overview.md)
#### [Elemental Live-Encoder](media-services-configure-elemental-live-encoder.md)
#### [FMLE-Encoder](media-services-configure-fmle-live-encoder.md)
#### [NewTek TriCaster-Encoder](media-services-configure-tricaster-live-encoder.md)
#### [Wirecast-Encoder](media-services-configure-wirecast-live-encoder.md)
### [Behandeln von Vorgängen mit langer Ausführungsdauer](media-services-dotnet-long-operations.md)
### [Spezifikation für die Liveerfassung von fragmentiertem MP4](media-services-fmp4-live-ingest-overview.md)

## [Schützen](media-services-content-protection-overview.md)
### [Konfigurieren von Content Protection im Portal](media-services-portal-protect-content.md)
### [Konfigurieren eines unverschlüsselten AES-128-Schlüssels für Ihren Datenstrom](media-services-protect-with-aes128.md)
### [Verwenden von REST für die Verschlüsselung von Inhalten mit der Speicherverschlüsselung](media-services-rest-storage-encryption.md)
### [Media Services PlayReady-Lizenzvorlage – Übersicht](media-services-playready-license-template-overview.md)
### [Übersicht über die Widevine-Lizenzvorlage](media-services-widevine-license-template-overview.md)
### [DRM-Lizenzbereitstellung](media-services-deliver-keys-and-licenses.md)
### [Übermitteln von Widevine-Lizenzen an Media Services mithilfe von Partnern](media-services-licenses-partner-integration.md)
#### [Bereitstellen von Widevine-Lizenzen für Media Services mithilfe von Axinom ](media-services-axinom-integration.md)
#### [Bereitstellen von Widevine-Lizenzen für Media Services mithilfe von castLabs](media-services-castlabs-integration.md)
### [Verwenden von dynamischer allgemeiner Verschlüsselung mit PlayReady und/oder Widevine.](media-services-protect-with-drm.md)
### [Geschütztes Streamen von HLS-Inhalten mit Apple FairPlay](media-services-protect-hls-with-fairplay.md)
### [Hybriddesign des DRM-Subsystems](hybrid-design-drm-sybsystem.md)
### [CENC mit Multi-DRM und Access Control](media-services-cenc-with-multidrm-access-control.md)
### Konfigurieren von Übermittlungsrichtlinien für Medienobjekte
#### [.NET](media-services-dotnet-configure-asset-delivery-policy.md)
#### [REST](media-services-rest-configure-asset-delivery-policy.md)
### Erstellen von ContentKeys
#### [.NET](media-services-dotnet-create-contentkey.md)
#### [REST](media-services-rest-create-contentkey.md)
### Konfigurieren einer Autorisierungsrichtlinie für Inhaltsschlüssel
#### [Portal](media-services-portal-configure-content-key-auth-policy.md)
#### [.NET](media-services-dotnet-configure-content-key-auth-policy.md)
#### [REST](media-services-rest-configure-content-key-auth-policy.md)

## [Analysieren](media-services-analytics-overview.md)
### [Medienanalyse mithilfe des Portals](media-services-portal-analyze.md)
### [Verarbeiten mit Indexer 2](media-services-process-content-with-indexer2.md)
### [Verarbeiten mit Indexer](media-services-index-content.md)
#### [Aufgabenvoreinstellung](indexer-task-preset.md)
### [Verarbeiten mit Hyperlapse](media-services-hyperlapse-content.md)
### [Verarbeiten mit Gesichtserkennung](media-services-face-and-emotion-detection.md)
### [Verarbeiten mit Bewegungserkennung](media-services-motion-detection.md)
### [Verarbeiten mit Face Redactor](media-services-face-redaction.md)
#### [Exemplarische Vorgehensweise für Face Redactor](media-services-redactor-walkthrough.md)
### [Verarbeiten mit Videominiaturansichten](media-services-video-summarization.md)
### [Verarbeiten mit OCR](media-services-video-optical-character-recognition.md)

## [Telemetrie konfigurieren](media-services-telemetry-overview.md)
###[.NET](media-services-dotnet-telemetry.md)
###[REST](media-services-rest-telemetry.md)

## Skalieren
### [Medienverarbeitung](media-services-scale-media-processing-overview.md)
#### [Portal](media-services-portal-scale-media-processing.md)
#### [.NET](media-services-dotnet-encoding-units.md)
#### [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
### Streamingendpunkte
#### [Portal](media-services-portal-scale-streaming-endpoints.md)

## [Bereitstellen von Inhalten](media-services-deliver-content-overview.md)
### [Dynamische Paketerstellung](media-services-dynamic-packaging-overview.md)
### [Übersicht über Filter und dynamische Manifeste](media-services-dynamic-manifest-overview.md)
#### [Erstellen von Filtern mit .NET](media-services-dotnet-dynamic-manifest.md)
#### [Erstellen von Filtern mit REST](media-services-rest-dynamic-manifest.md)
### [CDN-Cachingrichtlinie in der Media Services-Erweiterung](../cdn/cdn-caching-policy.md?toc=%2fazure%2fmedia-services%2ftoc.json)
### Veröffentlichen von Inhalten
#### [Portal](media-services-portal-publish.md)
#### [.NET](media-services-deliver-streaming-content.md)
#### [REST](media-services-rest-deliver-streaming-content.md)
### [Bereitstellen per Download](media-services-deliver-asset-download.md)
### [Failoverstreamingszenario](media-services-implement-failover.md)

## Nutzen
### [Wiedergeben von Medien mit vorhandenen Playern](media-services-playback-content-with-existing-players.md)
### [Wiedergeben von Medien mit Media Player](media-services-develop-video-players.md)
### Weitere Wiedergabeoptionen
#### [Windows Store-Anwendung mit Smooth Streaming](media-services-build-smooth-streaming-apps.md)
#### [HTML5-Anwendung mit DASH.js](media-services-embed-mpeg-dash-in-html5.md)
#### [Adobe Open Source Media Framework-Player](media-services-use-osmf-smooth-streaming-client-plugin.md)
### [Einfügen von Anzeigen auf Clientseite](media-services-inserting-ads-on-client-side.md)
### [Lizenzieren des Portierungskits für den Smooth Streaming-Client von Microsoft](media-services-sspk.md)

## Integrieren
### [Verwenden von Azure Functions mit Media Services](media-services-dotnet-how-to-use-azure-functions.md)
### [Beispiele für Azure Functions mit Media Services](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)

## Überwachen
### Prüfen des Auftragsstatus
#### [REST](media-services-rest-check-job-progress.md)
#### [Portal](media-services-portal-check-job-progress.md)
#### [.NET](media-services-check-job-progress.md)
### [Überwachen von Auftragsbenachrichtigungen mit dem Warteschlangenspeicher](media-services-dotnet-check-job-progress-with-queues.md)
### [Überwachen von Auftragsbenachrichtigungen mit Webhooks](media-services-dotnet-check-job-progress-with-webhooks.md)

## Problembehandlung
### [Häufig gestellte Fragen](media-services-frequently-asked-questions.md)
### [Anleitung zur Behandlung von Problemen bei Livestreaming](media-services-troubleshooting-live-streaming.md)
### [Fehlercodes](media-services-error-codes.md)
### [Wiederholungslogik](media-services-retry-logic-in-dotnet-sdk.md)

# Referenz
## [PowerShell (Resource Manager)](/powershell/module/azurerm.media)
## [PowerShell (Dienstverwaltung)](/powershell/module/azure/?view=azuresmps-3.7.0)
## [.NET](/dotnet/api/microsoft.windowsazure.mediaservices.client)
## [REST](/rest/api/media/mediaservice)  

# Ressourcen
## [Azure Media Services-Community](media-services-community.md)
## [Azure-Roadmap](https://azure.microsoft.com/roadmap/)
## [Preise](https://azure.microsoft.com/pricing/details/media-services/)
## [Versionshinweise](media-services-release-notes.md)
## [Videos](https://azure.microsoft.com/resources/videos/index/?services=media-services)
