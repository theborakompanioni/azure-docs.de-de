<properties 
	pageTitle="Verwenden von Azure Media Services zum Streamen von durch Apple FairPlay geschützten HLS-Inhalten" 
	description="Dieses Thema bietet einen Überblick und veranschaulicht, wie Sie Azure Media Services verwenden, um Ihre HLS-Inhalte (HTTP Live Streaming) dynamisch mit Apple FairPlay verschlüsseln. Es zeigt auch, wie Sie den Lizenzbereitstellungsdienst von Media Services verwenden, um FairPlay-Lizenzen an Clients zu übermitteln." 
	services="media-services" 
	documentationCenter="" 
	authors="Juliako" 
	manager="erikre" 
	editor=""/>

<tags 
	ms.service="media-services" 
	ms.workload="media" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
 	ms.date="05/11/2016"
	ms.author="juliako"/>

#Verwenden von Azure Media Services zum Streamen von durch Apple FairPlay geschützten HLS-Inhalten 

Mit Azure Media Services können Sie Ihre HLS-Inhalte (HTTP Live Streaming) unter Verwendung der folgenden Formate dynamisch verschlüsseln:

- **Unverschlüsselter Schlüssel mit AES-128-Umschlag**: Der gesamte Block wird mit dem **AES-128 CBC**-Modus verschlüsselt. Die Entschlüsselung des Streams wird von iOS- und OS X-Playern nativ unterstützt. [hier finden Sie weitere Informationen](media-services-protect-with-aes128.md)

- **Apple FairPlay**: Die einzelnen Audio- und Videosamples werden mit dem **AES-128 CBC**-Modus verschlüsselt. **FairPlay Streaming** (FPS) ist in die Gerätebetriebssysteme integriert und wird von iOS und Apple TV nativ unterstützt. Safari unter OS X ermöglicht FPS durch Unterstützung der EME-Schnittstelle (Encrypted Media Extensions).

	>[AZURE.NOTE]
	Die Verwendung von AMS zur Bereitstellung von HLS-verschlüsselten Inhalten mit FairPlay befindet sich zurzeit in der Vorschau.


Die folgende Abbildung veranschaulicht den Workflow für dynamische FairPlay-Verschlüsselung.

![Schützen mit FairPlay](./media/media-services-content-protection-overview/media-services-content-protection-with-fairplay.png)

Dieses Thema veranschaulicht, wie Sie Azure Media Services verwenden, um Ihre HLS-Inhalte dynamisch mit Apple FairPlay verschlüsseln. Es zeigt auch, wie Sie den Lizenzbereitstellungsdienst von Media Services verwenden, um FairPlay-Lizenzen an Clients zu übermitteln.

	
## Anforderungen und Überlegungen

- Folgendes ist erforderlich, wenn Sie AMS verwenden, um mit FairPlay verschlüsselte HLS-Inhalte sowie FairPlay-Lizenzen zu übermitteln.

	- Ein Azure-Konto. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](/pricing/free-trial/?WT.mc_id=A261C142F).
	- Media Services-Konto. Informationen zum Erstellen eines Media Services-Kontos finden Sie unter [Konto erstellen](media-services-create-account.md).
	- Registrieren Sie sich für das [Apple-Entwicklungsprogramm](https://developer.apple.com/).
	- Apple setzt voraus, dass der Inhaltsbesitzer über das [Bereitstellungspaket](https://developer.apple.com/contact/fps/) verfügt. Geben Sie an, dass Sie bereits KSM (Key Security Module) mit Azure Media Services implementiert haben und dass Sie das endgültige FPS-Paket anfordern. Im endgültigen FPS-Paket gibt es Anweisungen zum Generieren der Zertifizierung und zum Abrufen von ASK, den Sie zum Konfigurieren von FairPlay verwenden werden. 

	- Azure Media Services .NET SDK, Version **3.6.0** oder höher.

- Folgendes muss seitens der AMS-Schlüsselbereitstellung festgelegt werden:
	- **App Cert (AC)**: PFX-Datei mit dem privaten Schlüssel. Diese Datei wird vom Kunden erstellt und vom gleichen Kunden mit einem Kennwort verschlüsselt. 
		
	 	Wenn der Kunde die Richtlinie für die Schlüsselbereitstellung konfiguriert, muss er dieses Kennwort und die PFX-Datei im Base64-Format bereitstellen.

	- **App Cert-Kennwort**: Kennwort des Kunden zum Erstellen der PFX-Datei.
	- **App Cert-Kennwort-ID**: Der Kunde muss das Kennwort hochladen, auf die gleiche Weise wie andere AMS-Schlüssel und unter Verwendung des Enumerationswerts **ContentKeyType.FairPlayPfxPassword**. Als Ergebnis erhält der Kunde eine AMS-ID, die er in der Richtlinienoption für die Schlüsselbereitstellung benötigt.
	- **iv**: Zufälliger 16-Bytes-Wert, muss dem iv-Wert in der Richtlinie zur Übermittlung von Medienobjekten entsprechen. Der Kunde generiert den iv-Wert und gibt ihn in der Richtlinie zur Übermittlung von Medienobjekten sowie in der Richtlinienoption für die Schlüsselbereitstellung an. 
	- **ASK**: Ein ASK (Application Secret Key) wird erstellt, wenn Sie das Zertifikat über das Apple Developer-Portal generieren. Jedes Entwicklungsteam erhält einen eindeutigen ASK. Speichern Sie eine Kopie des ASK an einem sicheren Ort. Sie müssen den ASK später als FairPlayAsk für Azure Media Services konfigurieren. 
	-  **ASK-ID**: Wird abgerufen wird, wenn der Kunde ASK in AMS hochlädt. Der Kunde muss ASK mit dem **ContentKeyType.FairPlayASk**-Aufzählungswert hochladen. Dadurch wird die AMS-ID zurückgegeben, die Sie beim Festlegen der Schlüsselbereitstellungs-Richtlinienoption verwenden sollten.

- Folgendes muss seitens des FPS-Clients festgelegt werden:
 	- **App Cert (AC)**: CER-/DER-Datei mit dem öffentlichen Schlüssel, den das Betriebssystem zur Verschlüsselung der Nutzlast verwendet. AMS muss den Schlüssel kennen, da er vom Player benötigt wird. Der Schlüsselbereitstellungsdienst entschlüsselt den Schlüssel mithilfe des entsprechenden privaten Schlüssels.

- Um einen über FairPlay verschlüsselten Stream wiederzugeben, müssen Sie zuerst den echten ASK abrufen und dann ein echtes Zertifikat generieren. In diesem Prozess werden alle drei Teile erstellt:

	-  DER-Datei 
	-  PFX-Datei 
	-  Kennwort für die PFX-Datei
 
- Folgende Clients unterstützen HLS mit **AES-128 CBC**-Verschlüsselung: Safari unter OS X, Apple TV, iOS.

##Schritte zum Konfigurieren der dynamischen FairPlay-Verschlüsselung und des Lizenzbereitstellungsdiensts

Die folgenden allgemeinen Schritte müssen ausgeführt werden, wenn Sie Ihre Medienobjekte mit FairPlay schützen und dabei den Media Services-Lizenzbereitstellungsdienst sowie die dynamische Verschlüsselung verwenden möchten.

1. Erstellen eines Medienobjekts und Hochladen von Dateien in das Medienobjekt. 
1. Codieren eines Medienobjekts, das die Sammlung von MP4-Dateien mit adaptiver Bitrate enthält.
1. Erstellen eines Inhaltsschlüssels und Zuordnen des Schlüssels zum codierten Medienobjekt  
1. Konfigurieren der Autorisierungsrichtlinie des Inhaltsschlüssels. Wenn Sie die Autorisierungsrichtlinie für Inhaltsschlüssel erstellen, müssen Sie Folgendes angeben: 
	
	- Bereitstellungsmethode (in diesem Fall FairPlay) 
	- Konfiguration der FairPlay-Richtlinienoptionen; Informationen zum Konfigurieren von FairPlay finden Sie im Beispiel weiter unten in der ConfigureFairPlayPolicyOptions()-Methode
	
		>[AZURE.NOTE] In den meisten Fällen sollten Sie FairPlay-Richtlinienoptionen nur einmal konfigurieren, da Sie nur einen Satz aus Zertifizierung und ASK haben.
	- Einschränkungen (offen oder tokenbasiert) 
	- Informationen zum Typ der Schlüsselbereitstellung, der definiert, wie der Schlüssel an den Kunden übermittelt wird. 
	
2. Konfigurieren Sie die Übermittlungsrichtlinie für Medienobjekte. Die Richtlinienkonfiguration umfasst Folgendes:

	- Übermittlungsprotokoll (HLS) 
	- Typ der dynamischen Verschlüsselung (Common CBC Encryption) 
	- Lizenzerwerbs-URL 
	
	>[AZURE.NOTE]Wenn Sie einen Stream übermitteln möchten, der mit FairPlay und einem weiteren DRM-Mechanismus verschlüsselt wurde, müssen Sie separate Übermittlungsrichtlinien konfigurieren:
	>
	>- Eine IAssetDeliveryPolicy-Richtlinie zum Konfigurieren von DASH mit CENC (PlayReady + WideVine) und Smooth mit PlayReady 
	>- Eine weitere IAssetDeliveryPolicy-Richtlinie zum Konfigurieren von FairPlay für HLS

1. Erstellen eines "OnDemand"-Locators, um eine Streaming-URL zu erhalten.

##Verwenden der FairPlay-Schlüsselübermittlung nach Player-/Client-Apps

Kunden können Player-Apps mithilfe des iOS SDK entwickeln. Damit FairPlay-Inhalte wiedergegeben werden können, müssen Kunden das Lizenzaustauschprotokoll implementieren. Das Lizenzaustauschprotokoll wird nicht von Apple angegeben. Es hängt jeweils von der App ab, wie Schlüsselübermittlungsanforderungen gesendet werden. Der AMS-FairPlay-Schlüsselübermittlungsdienst erwartet das SPC als POST-Nachricht mit Verschlüsselung vom Typ „www-form-url“ im folgenden Format:

	spc=<Base64 encoded SPC>

>[AZURE.NOTE] Azure Media Player unterstützt standardmäßig keine FairPlay-Wiedergabe. Kunden benötigen den Beispiel-Player des Apple-Entwicklerkontos, um FairPlay-Wiedergabe unter MAC OSX zu erhalten.
 

##.NET-Beispiel


Das folgende Beispiel veranschaulicht die Funktionalität, die im Azure Media Services SDK für .NET Version 3.6.0 eingeführt wurde: die Möglichkeit, Azure Media Services zum Übermitteln von Inhalten zu verwenden, die mit FairPlay verschlüsselt wurden. Der folgende NuGet-Paketbefehl wurde zum Installieren des Pakets verwendet:

	PM> Install-Package windowsazure.mediaservices -Version 3.6.0


1. Erstellen Sie ein neues Konsolenprojekt.
1. Verwenden Sie NuGet, um das Azure Media Services .NET SDK zu installieren und hinzuzufügen.
2. Fügen Sie zusätzliche Verweise hinzu: System.Configuration.
2. Fügen Sie eine CONFIG-Datei hinzu, die den Kontonamen und Schlüsselinformationen enthält:
	
		<?xml version="1.0" encoding="utf-8"?>
		<configuration>
		    <startup> 
		        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
		    </startup>
			  <appSettings>
			
			    <add key="MediaServicesAccountName" value="AccountName"/>
			    <add key="MediaServicesAccountKey" value="AccountKey"/>
			
			    <add key="Issuer" value="http://testacs.com"/>
			    <add key="Audience" value="urn:test"/>
			  </appSettings>
		</configuration>

1. Abrufen von mindestens einer Streamingeinheit für den Streamingendpunkt, von dem aus Sie die Bereitstellung Ihrer Inhalte planen Weitere Informationen finden Sie unter [Konfigurieren von Streamingendpunkten](media-services-dotnet-get-started.md#configure-streaming-endpoint-using-the-portal).

1. Überschreiben Sie den Code in Ihrer Datei "Program.cs" mit dem in diesem Abschnitt gezeigten Code.
			
		
		using System;
		using System.Collections.Generic;
		using System.Configuration;
		using System.IO;
		using System.Linq;
		using System.Threading;
		using Microsoft.WindowsAzure.MediaServices.Client;
		using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
		using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
		using Microsoft.WindowsAzure.MediaServices.Client.FairPlay;
		using Newtonsoft.Json;
		using System.Security.Cryptography.X509Certificates;
		
		namespace DynamicEncryptionWithFairPlay
		{
		    class Program
		    {
		        // Read values from the App.config file.
		        private static readonly string _mediaServicesAccountName =
		            ConfigurationManager.AppSettings["MediaServicesAccountName"];
		        private static readonly string _mediaServicesAccountKey =
		            ConfigurationManager.AppSettings["MediaServicesAccountKey"];
		
		        private static readonly Uri _sampleIssuer =
		            new Uri(ConfigurationManager.AppSettings["Issuer"]);
		        private static readonly Uri _sampleAudience =
		            new Uri(ConfigurationManager.AppSettings["Audience"]);
		
		        // Field for service context.
		        private static CloudMediaContext _context = null;
		        private static MediaServicesCredentials _cachedCredentials = null;
		
		        private static readonly string _mediaFiles =
		            Path.GetFullPath(@"../..\Media");
		
		        private static readonly string _singleMP4File =
		            Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");
		
		        static void Main(string[] args)
		        {
		            // Create and cache the Media Services credentials in a static class variable.
		            _cachedCredentials = new MediaServicesCredentials(
		                            _mediaServicesAccountName,
		                            _mediaServicesAccountKey);
		            // Used the cached credentials to create CloudMediaContext.
		            _context = new CloudMediaContext(_cachedCredentials);
		
		            bool tokenRestriction = false;
		            string tokenTemplateString = null;
		
		            IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
		            Console.WriteLine("Uploaded asset: {0}", asset.Id);
		
		            IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
		            Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);
		
		            IContentKey key = CreateCommonCBCTypeContentKey(encodedAsset);
		            Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
		            Console.WriteLine("FairPlay License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay));
		            Console.WriteLine();
		
		            if (tokenRestriction)
		                tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
		            else
		                AddOpenAuthorizationPolicy(key);
		
		            Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
		            Console.WriteLine();
		
		            CreateAssetDeliveryPolicy(encodedAsset, key);
		            Console.WriteLine("Created asset delivery policy. \n");
		            Console.WriteLine();
		
		            if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
		            {
		                // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
		                // back into a TokenRestrictionTemplate class instance.
		                TokenRestrictionTemplate tokenTemplate =
		                    TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
		
		                // Generate a test token based on the the data in the given TokenRestrictionTemplate.
		                // Note, you need to pass the key id Guid because we specified 
		                // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
		                Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
		                string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey,
		                                                                        DateTime.UtcNow.AddDays(365));
		                Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
		                Console.WriteLine();
		            }
		
		            string url = GetStreamingOriginLocator(encodedAsset);
		            Console.WriteLine("Encrypted HLS URL: {0}/manifest(format=m3u8-aapl)", url);
		
		            Console.ReadLine();
		        }
		
		        static public IAsset UploadFileAndCreateAsset(string singleFilePath)
		        {
		            if (!File.Exists(singleFilePath))
		            {
		                Console.WriteLine("File does not exist.");
		                return null;
		            }
		
		            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
		            IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.None);
		
		            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));
		
		            Console.WriteLine("Created assetFile {0}", assetFile.Name);
		
		            var policy = _context.AccessPolicies.Create(
		                                    assetName,
		                                    TimeSpan.FromDays(30),
		                                    AccessPermissions.Write | AccessPermissions.List);
		
		            var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);
		
		            Console.WriteLine("Upload {0}", assetFile.Name);
		
		            assetFile.Upload(singleFilePath);
		            Console.WriteLine("Done uploading {0}", assetFile.Name);
		
		            locator.Delete();
		            policy.Delete();
		
		            return inputAsset;
		        }
		
		        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
		        {
		            var encodingPreset = "H264 Multiple Bitrate 720p";
		
		            IJob job = _context.Jobs.Create(String.Format("Encoding into Mp4 {0} to {1}",
		                                    inputAsset.Name,
		                                    encodingPreset));
		
		            var mediaProcessors =
		                _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder Standard")).ToList();
		
		            var latestMediaProcessor =
		                mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();
		
		            ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
		            encodeTask.InputAssets.Add(inputAsset);
		            encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset), 	AssetCreationOptions.StorageEncrypted);
		
		            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
		            job.Submit();
		            job.GetExecutionProgressTask(CancellationToken.None).Wait();
		
		            return job.OutputMediaAssets[0];
		        }
		
		        static public IContentKey CreateCommonCBCTypeContentKey(IAsset asset)
		        {
		            // Create HLS SAMPLE AES encryption content key
		            Guid keyId = Guid.NewGuid();
		            byte[] contentKey = GetRandomBuffer(16);
		
		            IContentKey key = _context.ContentKeys.Create(
		                                    keyId,
		                                    contentKey,
		                                    "ContentKey",
		                                    ContentKeyType.CommonEncryptionCbcs);
		
		            // Associate the key with the asset.
		            asset.ContentKeys.Add(key);
		
		            return key;
		        }
		
		
		        static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
		        {
		            // Create ContentKeyAuthorizationPolicy with Open restrictions 
		            // and create authorization policy          
		
		            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
		                    {
		                        new ContentKeyAuthorizationPolicyRestriction
		                        {
		                            Name = "Open",
		                            KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
		                            Requirements = null
		                        }
		                    };
		
		
		            // Configure FairPlay policy option.
		            string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();
		
		            IContentKeyAuthorizationPolicyOption FairPlayPolicy =
		                _context.ContentKeyAuthorizationPolicyOptions.Create("",
		                ContentKeyDeliveryType.FairPlay,
		                restrictions,
		                FairPlayConfiguration);
		
		
		            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
		                        ContentKeyAuthorizationPolicies.
		                        CreateAsync("Deliver Common CBC Content Key with no restrictions").
		                        Result;
		
		            contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);
		
		            // Associate the content key authorization policy with the content key.
		            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
		            contentKey = contentKey.UpdateAsync().Result;
		        }
		
		        public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
		        {
		            string tokenTemplateString = GenerateTokenRequirements();
		
		            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
		                    {
		                        new ContentKeyAuthorizationPolicyRestriction
		                        {
		                            Name = "Token Authorization Policy",
		                            KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
		                            Requirements = tokenTemplateString,
		                        }
		                    };
		
		            // Configure FairPlay policy option.
		            string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();
		
		
		            IContentKeyAuthorizationPolicyOption FairPlayPolicy =
		                _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
		                       ContentKeyDeliveryType.FairPlay,
		                       restrictions,
		                       FairPlayConfiguration);
		
		            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
		                        ContentKeyAuthorizationPolicies.
		                        CreateAsync("Deliver Common CBC Content Key with token restrictions").
		                        Result;
		
		            contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);
		
		            // Associate the content key authorization policy with the content key
		            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
		            contentKey = contentKey.UpdateAsync().Result;
		
		            return tokenTemplateString;
		        }
		
		        private static string ConfigureFairPlayPolicyOptions()
		        {
		            // For testing you can provide all zeroes for ASK bytes together with the cert from Apple FPS SDK. 
		            // However, for production you must use a real ASK from Apple bound to a real prod certificate.
		            byte[] askBytes = Guid.NewGuid().ToByteArray();
		            var askId = Guid.NewGuid();
		            // Key delivery retrieves askKey by askId and uses this key to generate the response.
		            IContentKey askKey = _context.ContentKeys.Create(
		                                    askId,
		                                    askBytes,
		                                    "askKey",
		                                    ContentKeyType.FairPlayASk);
		
		            //Customer password for creating the .pfx file.
		            string pfxPassword = "<customer password for creating the .pfx file>";
		            // Key delivery retrieves pfxPasswordKey by pfxPasswordId and uses this key to generate the response.
		            var pfxPasswordId = Guid.NewGuid();
		            byte[] pfxPasswordBytes = System.Text.Encoding.UTF8.GetBytes(pfxPassword);
		            IContentKey pfxPasswordKey = _context.ContentKeys.Create(
		                                    pfxPasswordId,
		                                    pfxPasswordBytes,
		                                    "pfxPasswordKey",
		                                    ContentKeyType.FairPlayPfxPassword);
		
		            // iv - 16 bytes random value, must match the iv in the asset delivery policy.
		            byte[] iv = Guid.NewGuid().ToByteArray();
		
		            //Specify the .pfx file created by the customer.
		            var appCert = new X509Certificate2("path to the .pfx file created by the customer", pfxPassword, X509KeyStorageFlags.Exportable);
		
		            string FairPlayConfiguration =
		                Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
		                    appCert,
		                    pfxPassword,
		                    pfxPasswordId,
		                    askId,
		                    iv);
		
		            return FairPlayConfiguration;
		        }
		
		        static private string GenerateTokenRequirements()
		        {
		            TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
		
		            template.PrimaryVerificationKey = new SymmetricVerificationKey();
		            template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
		            template.Audience = _sampleAudience.ToString();
		            template.Issuer = _sampleIssuer.ToString();
		            template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
		
		            return TokenRestrictionTemplateSerializer.Serialize(template);
		        }
		
		        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
		        {
		            var kdPolicy = _context.ContentKeyAuthorizationPolicies.Where(p => p.Id == key.AuthorizationPolicyId).Single();
		
		            var kdOption = kdPolicy.Options.Single(o => o.KeyDeliveryType == ContentKeyDeliveryType.FairPlay);
		
		            FairPlayConfiguration configFP = JsonConvert.DeserializeObject<FairPlayConfiguration>(kdOption.KeyDeliveryConfiguration);
		
		            // Get the FairPlay license service URL.
		            Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay);
		
					// The reason the below code replaces "https://" with "skd://" is because
					// in the IOS player sample code which you obtained in Apple developer account, 
					// the player only recognizes a Key URL that starts with skd://. 
					// However, if you are using a customized player, 
					// you can choose whatever protocol you want. 
					// For example, "https". 

		            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
		                new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
		                {
		                    {AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, acquisitionUrl.ToString().Replace("https://", "skd://")},
		                    {AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs, configFP.ContentEncryptionIV}
		                };
		
		            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
		                    "AssetDeliveryPolicy",
		                AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
		                AssetDeliveryProtocol.HLS,
		                assetDeliveryPolicyConfiguration);
		
		            // Add AssetDelivery Policy to the asset
		            asset.DeliveryPolicies.Add(assetDeliveryPolicy);
		
		        }
		
		
		        /// <summary>
		        /// Gets the streaming origin locator.
		        /// </summary>
		        /// <param name="assets"></param>
		        /// <returns></returns>
		        static public string GetStreamingOriginLocator(IAsset asset)
		        {
		
		            // Get a reference to the streaming manifest file from the  
		            // collection of files in the asset. 
		
		            var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
		                                         EndsWith(".ism")).
		                                         FirstOrDefault();
		
		            // Create a 30-day readonly access policy. 
		            IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
		                TimeSpan.FromDays(30),
		                AccessPermissions.Read);
		
		            // Create a locator to the streaming content on an origin. 
		            ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
		                policy,
		                DateTime.UtcNow.AddMinutes(-5));
		
		            // Create a URL to the manifest file. 
		            return originLocator.Path + assetFile.Name;
		        }
		
		        static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
		        {
		            Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
		                ((IJob)sender).Name,
		                e.CurrentState,
		                DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
		        }
		
		        static private byte[] GetRandomBuffer(int length)
		        {
		            var returnValue = new byte[length];
		
		            using (var rng =
		                new System.Security.Cryptography.RNGCryptoServiceProvider())
		            {
		                rng.GetBytes(returnValue);
		            }
		
		            return returnValue;
		        }
		    }
		}


##Nächste Schritte: Media Services-Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Feedback geben

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!---HONumber=AcomDC_0518_2016-->