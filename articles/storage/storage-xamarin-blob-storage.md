<properties
	pageTitle="Verwenden des Blobspeichers mit Xamarin | Microsoft Azure"
	description="Die Azure Storage-Clientbibliothek für Xamarin ermöglicht Entwicklern das Erstellen von iOS-, Android- und Windows Store-Apps mit ihren nativen Benutzeroberflächen. In diesem Tutorial wird erläutert, wie mithilfe von Xamarin eine Anwendung erstellt wird, in der der Azure-Blobspeicher verwendet wird."
	services="storage"
	documentationCenter="xamarin"
	authors="micurd"
	manager="jahogg"
	editor="tysonn"/>

<tags
	ms.service="storage"
	ms.workload="storage"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/31/2016"
	ms.author="micurd;tamram"/>

# Verwenden des Blobspeichers mit Xamarin

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## Übersicht

Xamarin ermöglicht Entwicklern die Verwendung einer gemeinsamen C#-Codebasis zum Erstellen von iOS-, Android- und Windows Store-Apps mit ihren systemeigenen Benutzeroberflächen. In diesem Tutorial wird erläutert, wie Sie den Azure-Blobspeicher mit einer Xamarin-Anwendung verwenden können. Wenn Sie vor dem Einstieg in den Code mehr über Azure Storage erfahren möchten, lesen Sie [Einführung in Microsoft Azure Storage](storage-introduction.md).

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## Erstellen einer neuen Xamarin-Anwendung

Für diese ersten Schritte erstellen wir eine App, die auf Android, iOS und Windows abzielt. Diese App erstellt einfach einen Container und lädt ein Blob in diesen Container hoch. Wir verwenden Visual Studio unter Windows für diese ersten Schritte, aber beim Erstellen einer App mit Xamarin Studio unter Mac OS können Sie ähnlich vorgehen.

Führen Sie die folgenden Schritte aus, um die Anwendung zu erstellen:

1. Falls noch nicht geschehen, laden Sie [Xamarin für Visual Studio](https://www.xamarin.com/download) herunter, und installieren Sie die Anwendung.
2. Öffnen Sie Visual Studio, und erstellen Sie eine leere App (nativ freigegeben): **Datei > Neu > Projekt > Plattformübergreifend > Leere App (Nativ freigegeben)**.
3. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf Ihre Lösung, und wählen Sie **NuGet-Pakete verwalten** aus. Suchen Sie nach **WindowsAzure.Storage**, und installieren Sie die neueste stabile Version für alle Projekte in der Lösung.
4. Erstellen Sie Ihr Projekt, und führen Sie es aus.

Es sollte nun eine Anwendung verfügbar sein, in der Sie auf eine Schaltfläche klicken können, die einen Zähler schrittweise heraufsetzt.

> [AZURE.NOTE] Die Azure Storage-Clientbibliothek für Xamarin funktioniert auch für Xamarin.Forms-Anwendungen.

## Erstellen des Containers und Hochladen des Blobs

Als Nächstes fügen Sie der gemeinsam genutzten Klasse `MyClass.cs` Code hinzu, die einen Container erstellt und ein Blob in diesen Container lädt. `MyClass.cs` sollte wie folgt aussehen:

	using Microsoft.WindowsAzure.Storage;
	using Microsoft.WindowsAzure.Storage.Blob;
	using System.Threading.Tasks;

	namespace XamarinApp
	{
		public class MyClass
		{
			public MyClass ()
			{
			}

		    public static async Task createContainerAndUpload()
		    {
		        // Retrieve storage account from connection string.
		        CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here");

		        // Create the blob client.
		        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

		        // Retrieve reference to a previously created container.
		        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

				// Create the container if it doesn't already exist.
            	await container.CreateIfNotExistsAsync();

		        // Retrieve reference to a blob named "myblob".
		        CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

		        // Create the "myblob" blob with the text "Hello, world!"
		        await blockBlob.UploadTextAsync("Hello, world!");
		    }
		}
	}

Stellen Sie sicher, dass Sie „Your\_account\_name\_here“ und „Your\_account\_key\_here“ durch Ihren tatsächlichen Kontonamen und Kontoschlüssel ersetzen. Anschließend können Sie diese gemeinsam genutzte Klasse in Ihrer iOS-, Android- und Windows Phone-Anwendung verwenden. Sie können jedem Projekt einfach `MyClass.createContainerAndUpload()` hinzufügen. Beispiel:

### XamarinApp.Droid > MainActivity.cs

	using Android.App;
	using Android.Widget;
	using Android.OS;

	namespace XamarinApp.Droid
	{
		[Activity (Label = "XamarinApp.Droid", MainLauncher = true, Icon = "@drawable/icon")]
		public class MainActivity : Activity
		{
			int count = 1;

			protected override async void OnCreate (Bundle bundle)
			{
				base.OnCreate (bundle);

				// Set our view from the "main" layout resource
				SetContentView (Resource.Layout.Main);

				// Get our button from the layout resource,
				// and attach an event to it
				Button button = FindViewById<Button> (Resource.Id.myButton);

				button.Click += delegate {
					button.Text = string.Format ("{0} clicks!", count++);
				};

	      	  await MyClass.createContainerAndUpload();
			}
		}
	}

### XamarinApp.iOS > ViewController.cs

	using System;
	using UIKit;

	namespace XamarinApp.iOS
	{
		public partial class ViewController : UIViewController
		{
			int count = 1;

			public ViewController (IntPtr handle) : base (handle)
			{
			}

			public override async void ViewDidLoad ()
			{
				base.ViewDidLoad ();
				// Perform any additional setup after loading the view, typically from a nib.
				Button.AccessibilityIdentifier = "myButton";
				Button.TouchUpInside += delegate {
					var title = string.Format ("{0} clicks!", count++);
					Button.SetTitle (title, UIControlState.Normal);
				};

	            await MyClass.createContainerAndUpload();
	    	}

			public override void DidReceiveMemoryWarning ()
			{
				base.DidReceiveMemoryWarning ();
				// Release any cached data, images, etc that aren't in use.
			}
		}
	}

### XamarinApp.WinPhone > MainPage.xaml > MainPage.Xaml.cs

	using Windows.UI.Xaml.Controls;
	using Windows.UI.Xaml.Navigation;

	// The Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=391641

	namespace XamarinApp.WinPhone
	{
	    /// <summary>
	    /// An empty page that can be used on its own or navigated to within a Frame.
	    /// </summary>
	    public sealed partial class MainPage : Page
	    {
	        int count = 1;

	        public MainPage()
	        {
	            this.InitializeComponent();

	            this.NavigationCacheMode = NavigationCacheMode.Required;
	        }

	        /// <summary>
	        /// Invoked when this page is about to be displayed in a Frame.
	        /// </summary>
	        /// <param name="e">Event data that describes how this page was reached.
	        /// This parameter is typically used to configure the page.</param>
	        protected override async void OnNavigatedTo(NavigationEventArgs e)
	        {
	            // TODO: Prepare page for display here.

	            // TODO: If your application contains multiple pages, ensure that you are
	            // handling the hardware Back button by registering for the
	            // Windows.Phone.UI.Input.HardwareButtons.BackPressed event.
	            // If you are using the NavigationHelper provided by some templates,
	            // this event is handled for you.
	            Button.Click += delegate {
	                var title = string.Format("{0} clicks!", count++);
	                Button.Content = title;
	            };

	            await MyClass.createContainerAndUpload();
	        }
	    }
	}


## Ausführen der Anwendung

Sie können diese Anwendung jetzt in einem Android- oder Windows Phone-Emulator ausführen. Sie können diese Anwendung auch in einem iOS-Emulator ausführen, aber dazu benötigen Sie einen Mac. Spezifische Anweisungen hierzu finden Sie in der Dokumentation zum [Verbinden von Visual Studio mit einem Mac](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/).

Wenn Sie Ihre App ausführen, erstellt sie den Container `mycontainer` in Ihrem Speicherkonto. Es sollte das Blob `myblob` mit dem Text `Hello, world!` enthalten. Sie können dies mit dem [Microsoft Azure-Speicher-Explorer](http://storageexplorer.com/) überprüfen.

## Nächste Schritte

In diesen ersten Schritten haben Sie gelernt, wie Sie eine plattformübergreifende Anwendung in Xamarin erstellen, die Azure Storage verwendet. Diese ersten Schritten konzentrieren sich speziell auf ein Szenario in Blob Storage. Allerdings können Sie sie nicht nur auf Blob Storage, sondern auch auf Table Storage, File Storage und Queue Storage anwenden. Weitere Informationen finden Sie in den folgenden Artikeln:
- [Erste Schritte mit Azure Blob Storage mit .NET](storage-dotnet-how-to-use-blobs.md)
- [Erste Schritte mit Azure Table Storage mit .NET](storage-dotnet-how-to-use-tables.md)
- [Erste Schritte mit Azure Queue Storage mit .NET](storage-dotnet-how-to-use-queues.md)
- [Erste Schritte mit Azure File Storage unter Windows](storage-dotnet-how-to-use-files.md)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

<!---HONumber=AcomDC_0921_2016-->