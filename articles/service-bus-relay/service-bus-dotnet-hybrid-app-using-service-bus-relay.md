<properties
	pageTitle="Hybride lokale/Cloudanwendung (.NET) | Microsoft Azure"
	description="Erfahren Sie, wie eine lokale/Cloud-.NET-Hybridanwendung mithilfe von Microsoft Azure Service Bus Relay erstellen."
	services="service-bus-relay"
	documentationCenter=".net"
	authors="sethmanheim"
	manager="timlt"
	editor=""/>

<tags
	ms.service="service-bus-relay"
	ms.workload="tbd"
	ms.tgt_pltfrm="na"
	ms.devlang="dotnet"
	ms.topic="hero-article"
	ms.date="09/16/2016"
	ms.author="sethm"/>

# Hybride lokale/Cloud-.NET-Anwendung mit Azure Service Bus Relay

## Einführung

In diesem Artikel wird beschrieben, wie Sie mit Microsoft Azure und Visual Studio eine Hybridcloudanwendung erstellen. Im Tutorial wird davon ausgegangen, dass Sie noch keine Erfahrung mit der Verwendung von Azure haben. In weniger als 30 Minuten verfügen Sie über eine Anwendung, die verschiedene Microsoft Azure-Ressourcen nutzt und aktiv in der Cloud ausgeführt wird.

Sie erhalten Informationen zu folgenden Themen:

-   Erstellen oder Anpassen eines vorhandenen Webdiensts zur Verwendung durch eine Weblösung
-   Verwenden des Azure Service Bus Relay-Diensts zum Freigeben von Daten zwischen einer Azure-Anwendung und einem an einem anderen Ort gehosteten Webdienst

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## Unterstützung hybrider Lösungen durch Service Bus Relay

Geschäftslösungen bestehen normalerweise aus einer Kombination von benutzerdefiniertem Code, der geschrieben wurde, um neue und individuelle Geschäftsanforderungen in Angriff zu nehmen und vorhandene Funktionen, die von bereits verwendeten Lösungen und Systemen bereitgestellt werden, zu integrieren.

Lösungsarchitekten setzen die Cloud inzwischen ein, um Skalierungsanforderungen leichter bewältigen zu können und die Betriebskosten zu senken. Dabei stellen sie fest, dass sich vorhandene Dienstressourcen, die sie als Bausteine für ihre Lösungen nutzen möchten, innerhalb der Unternehmensfirewall befinden und somit nicht problemlos von der Cloudlösung darauf zugegriffen werden kann. Viele interne Dienste werden nicht so erstellt oder gehostet, dass sie einfach am Rand des Unternehmensnetzwerks verfügbar gemacht werden können.

Service Bus Relay wurde konzipiert, damit vorhandene WCF (Windows Communication Foundation)-Webdienste sicher von Lösungen aufgerufen werden können, die sich außerhalb des Unternehmensumkreises befinden, ohne dass aufwändige Änderungen an der Netzwerkinfrastruktur des Unternehmens ausgeführt werden müssen. Service Bus Relay-Dienste dieser Art werden weiterhin in der vorhandenen Umgebung gehostet, das Überprüfen auf eingehende Sitzungen und Anforderungen wird jedoch an den in der Cloud gehosteten Servicebus delegiert. Dieser schützt die Dienste durch die SAS-Authentifizierung ([Shared Access Signature](../service-bus/service-bus-sas-overview.md)) außerdem vor unbefugten Zugriffen.

## Lösungszenario

In diesem Lernprogramm erstellen Sie eine ASP.NET-Website, mit der Sie eine Produktliste auf der Produktbestandsseite anzeigen können.

![][0]

In diesem Lernprogramm wird davon ausgegangen, dass Sie über Produktinformationen in einem vorhandenen lokalen System verfügen, auf das über Service Bus Relay zugegriffen werden kann. Dies wird durch einen Webdienst simuliert, der in einer einfachen Konsolenanwendung ausgeführt und durch einen im Arbeitsspeicher befindlichen Produktsatz unterstützt wird. Sie können diese Konsolenanwendung auf Ihrem eigenen Computer ausführen und die Webrolle in Azure bereitstellen. Dabei werden Sie feststellen, dass die im Azure-Datencenter ausgeführte Webrolle tatsächlich Aufrufe an Ihren Computer sendet, obwohl dieser sich mit ziemlicher Sicherheit hinter mindestens einer Firewall und einer NAT (Network Address Translation)-Ebene befindet.

Im Folgenden wird ein Bildschirmfoto der Startseite der vollständigen Webanwendung dargestellt.

![][1]

## Einrichten der Entwicklungsumgebung

Bevor Sie mit der Entwicklung von Azure-Anwendungen beginnen können, müssen Sie über die entsprechenden Tools verfügen und die Entwicklungsumgebung einrichten.

1.  Installieren Sie das Azure-SDK für .NET über die Seite [Tools und SDK herunterladen][].

2. 	Klicken Sie für Ihre Visual Studio-Version auf **Installieren des SDK**. Für die Schritte in diesem Tutorial wird Visual Studio 2015 verwendet.

4.  Wenn Sie aufgefordert werden, das Installationsprogramm auszuführen oder zu speichern, klicken Sie auf **Ausführen**.

5.  Klicken Sie im **Webplattform-Installer** auf **Installieren**, und setzen Sie die Installation fort.

6.  Nach Abschluss der Installation haben Sie alles zur Hand, was Sie benötigen, um mit der Entwicklung der App zu beginnen. Das SDK enthält Tools, mit denen Sie ganz leicht Azure-Anwendungen in Visual Studio entwickeln können. Wenn Visual Studio nicht installiert ist, wird außerdem die kostenlose Version Visual Studio Express vom SDK installiert.

## Erstellen eines Namespace

Um mit der Verwendung von Service Bus-Features in Azure beginnen zu können, müssen Sie zuerst einen Dienstnamespace erstellen. Ein Namespace ist ein Bereichscontainer für die Adressierung von Service Bus-Ressourcen innerhalb Ihrer Anwendung.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## Erstellen eines lokalen Servers

Zunächst erstellen Sie ein (falsches) lokales Produktkatalogsystem. Dies ist relativ einfach; Sie können es sich als ein echtes lokales Produktkatalogsystem mit einer vollständigen Dienstoberfläche vorstellen, das integriert werden soll.

Bei diesem Projekt handelt es sich um eine Visual Studio-Konsolenanwendung, bei dem das [Azure Service Bus-NuGet-Paket](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) verwendet wird, um die Service Bus-Bibliotheken und -Konfigurationseinstellungen einzubinden.

### Erstellen des Projekts

1.  Starten Sie Microsoft Visual Studio mit Administratorrechten. Klicken Sie mit der rechten Maustaste auf das Programmsymbol von **Visual Studio**, und klicken Sie anschließend auf **Als Administrator ausführen**, um Visual Studio mit Administratorrechten zu starten.

2.  Klicken Sie in Visual Studio im Menü **Datei** auf **Neu** und anschließend auf **Projekt**.

3.  Klicken Sie in **Installierte Vorlagen** unter **Visual C#** auf **Konsolenanwendung**. Geben Sie in das Feld **Name** den Namen **ProductsServer** ein:

    ![][11]

4.  Klicken Sie auf **OK**, um das Projekt **ProductsServer** zu erstellen.

7.  Wenn Sie den NuGet-Paket-Manager für Visual Studio bereits installiert haben, fahren Sie mit dem nächsten Schritt fort. Besuchen Sie andernfalls [NuGet][], und klicken Sie auf [Install NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c). Befolgen Sie die Anweisungen, um den NuGet-Paket-Manager zu installieren, und starten Sie Visual Studio dann neu.

7.  Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **ProductsServer**, und klicken Sie anschließend auf **NuGet-Pakete verwalten**.

8.  Klicken Sie auf die Registerkarte **Durchsuchen**, und suchen Sie nach `Microsoft Azure Service Bus`. Klicken Sie auf **Installieren**, und akzeptieren Sie die Nutzungsbedingungen.

    ![][13]

    Beachten Sie, dass nun auf die erforderlichen Clientassemblys verwiesen wird.

9.  Fügen Sie eine neue Klasse für den Produktvertrag hinzu. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **ProductsServer**. Klicken Sie auf **Hinzufügen**, und dann auf **Klasse**.

10. Geben Sie in das Feld **Name** den Namen **ProductsContract.cs** ein. Klicken Sie anschließend auf **Hinzufügen**.

11. Ersetzen Sie in **ProductsContract.cs** die Namespacedefinition durch den folgenden Code, mit dem der Vertrag für den Dienst definiert wird.

	```
	namespace ProductsServer
	{
	    using System.Collections.Generic;
	    using System.Runtime.Serialization;
	    using System.ServiceModel;
	
	    // Define the data contract for the service
	    [DataContract]
	    // Declare the serializable properties.
	    public class ProductData
	    {
	        [DataMember]
	        public string Id { get; set; }
	        [DataMember]
	        public string Name { get; set; }
	        [DataMember]
	        public string Quantity { get; set; }
	    }
	
	    // Define the service contract.
	    [ServiceContract]
	    interface IProducts
	    {
	        [OperationContract]
	        IList<ProductData> GetProducts();
	
	    }
	
	    interface IProductsChannel : IProducts, IClientChannel
	    {
	    }
	}
	```

12. Ersetzen Sie in "Program.cs" die Namespacedefinition durch den folgenden Code, mit dem der Profildienst und der dafür vorgesehene Host hinzugefügt werden.

	```
	namespace ProductsServer
	{
	    using System;
	    using System.Linq;
	    using System.Collections.Generic;
	    using System.ServiceModel;
	
	    // Implement the IProducts interface.
	    class ProductsService : IProducts
	    {
	
	        // Populate array of products for display on website
	        ProductData[] products =
	            new []
	                {
	                    new ProductData{ Id = "1", Name = "Rock",
	                                     Quantity = "1"},
	                    new ProductData{ Id = "2", Name = "Paper",
	                                     Quantity = "3"},
	                    new ProductData{ Id = "3", Name = "Scissors",
	                                     Quantity = "5"},
	                    new ProductData{ Id = "4", Name = "Well",
	                                     Quantity = "2500"},
	                };
	
	        // Display a message in the service console application
	        // when the list of products is retrieved.
	        public IList<ProductData> GetProducts()
	        {
	            Console.WriteLine("GetProducts called.");
	            return products;
	        }
	
	    }
	
	    class Program
	    {
	        // Define the Main() function in the service application.
	        static void Main(string[] args)
	        {
	            var sh = new ServiceHost(typeof(ProductsService));
	            sh.Open();
	
	            Console.WriteLine("Press ENTER to close");
	            Console.ReadLine();
	
	            sh.Close();
	        }
	    }
	}
	```

13. Doppelklicken Sie im Projektmappen-Explorer auf die Datei **App.config**, um sie im Visual Studio-Editor zu öffnen. Fügen Sie am Ende des **&lt;system.ServiceModel&gt;**-Elements (aber noch innerhalb von &lt;system.ServiceModel&gt;) den folgenden XML-Code hinzu. Ersetzen Sie dabei *yourServiceNamespace* durch den Namen Ihres Namespace und *yourKey* durch den SAS-Schlüssel, den Sie zuvor aus dem Portal abgerufen haben:

    ```
    <system.serviceModel>
	...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```
14. Ersetzen Sie in „App.config“ im **&lt;appSettings&gt;**-Element den Wert der Verbindungszeichenfolge durch die Verbindungszeichenfolge, die Sie zuvor aus dem Portal abgerufen haben.

	```
	<appSettings>
   	<!-- Service Bus specific app settings for messaging connections -->
   	<add key="Microsoft.ServiceBus.ConnectionString"
	       value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
	</appSettings>
	```

14. Drücken Sie STRG+UMSCHALT+B, oder klicken Sie im Menü **Build** auf **Projektmappe erstellen**, um die Anwendung zu erstellen und Ihre bisherige Arbeit zu überprüfen.

## Erstellen einer ASP.NET-Anwendung

In diesem Abschnitt erstellen Sie eine einfache ASP.NET-Anwendung, in der von Ihrem Produktdienst abgerufene Daten angezeigt werden.

### Erstellen des Projekts

1.  Stellen Sie sicher, dass Visual Studio mit Administratorberechtigungen ausgeführt wird.

2.  Klicken Sie in Visual Studio im Menü **Datei** auf **Neu** und anschließend auf **Projekt**.

3.  Klicken Sie in **Installierte Vorlagen** unter **Visual C#** auf **ASP.NET-Webanwendung**. Nennen Sie das Projekt **ProductsPortal**. Klicken Sie dann auf **OK**.

    ![][15]

4.  Klicken Sie in der Liste **Vorlage auswählen** auf **MVC**.

6.  Aktivieren Sie das Kontrollkästchen **In der Cloud hosten**.

    ![][16]

5. Klicken Sie auf die Schaltfläche **Authentifizierung ändern**. Klicken Sie im Dialogfeld **Authentifizierung ändern** auf **Keine Authentifizierung** und dann auf **OK**. Für dieses Tutorial stellen Sie eine App bereit, für die keine Benutzeranmeldung erforderlich ist.

	![][18]

6. 	Vergewissern Sie sich im Dialogfeld **Neues ASP.NET-Projekt** im Abschnitt **Microsoft Azure**, dass **In der Cloud hosten** aktiviert und in der Dropdownliste die Option **App Service** ausgewählt ist.

	![][19]

7. Klicken Sie auf **OK**.

8. Als Nächstes müssen Sie die Azure-Ressourcen für eine neue Web-App konfigurieren. Führen Sie alle Schritte im Abschnitt [Konfigurieren der Azure-Ressourcen für eine neue Web-App](../app-service-web/web-sites-dotnet-get-started.md#configure-azure-resources-for-a-new-web-app) aus. Kehren Sie anschließend zu diesem Tutorial zurück, und fahren Sie mit dem nächsten Schritt fort.

5.  Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **Modelle**, und klicken Sie anschließend auf **Hinzufügen** und dann auf **Klasse**. Geben Sie in das Feld **Name** den Namen **Product.cs** ein. Klicken Sie anschließend auf **Hinzufügen**.

    ![][17]

### Ändern der Webanwendung

1.  Ersetzen Sie in der Datei "Product.cs" in Visual Studio die vorhandene Namespacedefinition durch den folgenden Code.

	```
	// Declare properties for the products inventory.
 	namespace ProductsWeb.Models
	{
    	public class Product
    	{
    	    public string Id { get; set; }
    	    public string Name { get; set; }
    	    public string Quantity { get; set; }
    	}
	}
	```

2.  Erweitern Sie im Projektmappen-Explorer den Ordner **Controller**, und doppelklicken Sie auf die Datei **HomeController.cs**, um sie in Visual Studio zu öffnen.

3. Ersetzen Sie in der Datei **HomeController.cs** die vorhandene Namespacedefinition durch den folgenden Code:

	```
	namespace ProductsWeb.Controllers
	{
	    using System.Collections.Generic;
	    using System.Web.Mvc;
	    using Models;
	
	    public class HomeController : Controller
	    {
	        // Return a view of the products inventory.
	        public ActionResult Index(string Identifier, string ProductName)
	        {
	            var products = new List<Product>
	                {new Product {Id = Identifier, Name = ProductName}};
	            return View(products);
	        }
	     }
	}
	```

3.  Erweitern Sie im Projektmappen-Explorer den Ordner „Views\\Shared“, und doppelklicken Sie dann auf **\_Layout.cshtml**, um die Datei im Visual Studio-Editor zu öffnen.

5.  Ändern Sie alle Vorkommnisse von **My ASP.NET Application** in **LITWARE's Products**.

6. Entfernen Sie die Links **Home**, **About** und **Contact**. Löschen Sie im folgenden Beispiel den hervorgehobenen Code.

	![][41]

7.  Erweitern Sie im Projektmappen-Explorer den Ordner „Views\\Home“, und doppelklicken Sie dann auf **Index.cshtml**, um die Datei im Visual Studio-Editor zu öffnen. Ersetzen Sie den gesamten Inhalt der Datei durch den folgenden Code.

	```
	@model IEnumerable<ProductsWeb.Models.Product>
	
	@{
	 		ViewBag.Title = "Index";
	}
	
	<h2>Prod Inventory</h2>
	
	<table>
	  		<tr>
	      		<th>
	          		@Html.DisplayNameFor(model => model.Name)
	      		</th>
	              <th></th>
	      		<th>
	          		@Html.DisplayNameFor(model => model.Quantity)
	      		</th>
	  		</tr>
	
	@foreach (var item in Model) {
	  		<tr>
	      		<td>
	          		@Html.DisplayFor(modelItem => item.Name)
	      		</td>
	      		<td>
	          		@Html.DisplayFor(modelItem => item.Quantity)
	      		</td>
	  		</tr>
	}
	
	</table>
	```

9.  Drücken Sie STRG+UMSCHALT+B, um das Projekt zu erstellen und Ihre bisherige Arbeit zu überprüfen.


### Lokales Ausführen der App

Führen Sie die Anwendung aus, um sicherzustellen, dass sie funktioniert.

1.  Stellen Sie sicher, dass **ProductsPortal** das aktive Projekt ist. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Als Startprojekt festlegen** aus.
2.  Drücken Sie in Visual Studio die Taste F5.
3.  Die Anwendung sollte ausgeführt und in einem Browser angezeigt werden.

    ![][21]

## Zusammensetzen der Einzelteile

Im nächsten Schritt wird der lokale Produktserver mit der ASP.NET-Webanwendung zusammengeführt.

1.  Falls noch nicht geschehen, öffnen Sie in Visual Studio erneut das Projekt **ProductsPortal**, das Sie im Abschnitt [Erstellen einer ASP.NET-Anwendung](#create-an-aspnet-application) erstellt haben.

2.  Fügen Sie das NuGet-Paket den Projektverweisen hinzu, wie im Schritt „Erstellen eines lokalen Servers“ beschrieben. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **ProductsPortal**, und klicken Sie anschließend auf **NuGet-Pakete verwalten**.

3.  Suchen Sie nach „Service Bus“, und wählen Sie das Element **Microsoft Azure Service Bus** aus. Schließen Sie die Installation ab, und schließen Sie das Dialogfeld.

4.  Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **ProductsPortal**. Klicken Sie auf **Hinzufügen** und dann auf **Vorhandenes Element**.

5.  Navigieren Sie im Konsolenprojekt **ProductsServer** zur Datei **ProductsContract.cs**. Klicken Sie, um "ProductsContract.cs" zu markieren. Klicken Sie auf den Pfeil nach unten neben **Hinzufügen** und dann auf **Als Link hinzufügen**.

	![][24]

6.  Öffnen Sie nun die Datei **HomeController.cs** im Visual Studio-Editor, und ersetzen Sie die Namespace-Definition durch den folgenden Code. Achten Sie darauf, *yourServiceNamespace* durch den Namen Ihres Dienstnamespaces und *yourKey* durch Ihren SAS-Schlüssel zu ersetzen. Der Client kann daraufhin den lokalen Dienst aufrufen und das Ergebnis des Aufrufs zurückgeben.

	```
	namespace ProductsWeb.Controllers
	{
	    using System.Linq;
	    using System.ServiceModel;
	    using System.Web.Mvc;
	    using Microsoft.ServiceBus;
	    using Models;
	    using ProductsServer;
	
	    public class HomeController : Controller
	    {
	        // Declare the channel factory.
	        static ChannelFactory<IProductsChannel> channelFactory;
	
	        static HomeController()
	        {
	            // Create shared access signature token credentials for authentication.
	            channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
	                "sb://yourServiceNamespace.servicebus.windows.net/products");
	            channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
	                TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
	                    "RootManageSharedAccessKey", "yourKey") });
	        }
	
	        public ActionResult Index()
	        {
	            using (IProductsChannel channel = channelFactory.CreateChannel())
	            {
	                // Return a view of the products inventory.
	                return this.View(from prod in channel.GetProducts()
	                                 select
	                                     new Product { Id = prod.Id, Name = prod.Name,
	                                         Quantity = prod.Quantity });
	            }
	        }
	    }
	}
	```

7.  Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe **ProductsPortal** (klicken Sie auf die Projektmappe, nicht auf das Projekt). Klicken Sie auf **Hinzufügen** und anschließend auf **Vorhandenes Projekt**.

8.  Navigieren Sie zum Projekt **ProductsServer**, und doppelklicken Sie dann auf die Projektmappendatei **ProductsServer.csproj**, um sie hinzuzufügen.

9.  **ProductsServer** muss ausgeführt werden, damit die Daten unter **ProductsPortal** angezeigt werden. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe **ProductsPortal**, und klicken Sie dann auf **Eigenschaften**. Das Dialogfeld **Eigenschaftenseiten** wird angezeigt.

10. Klicken Sie auf der linken Seite auf **Startprojekt**. Klicken Sie auf der rechten Seite auf **Mehrere Startprojekts**. Vergewissern Sie sich, dass **ProductsServer** und **ProductsPortal** in dieser Reihenfolge angezeigt werden und jeweils **Start** als Aktion festgelegt ist.

      ![][25]

11. Klicken Sie im Dialogfeld **Eigenschaften** auf der linken Seite auf **Projektabhängigkeiten**.

12. Klicken Sie in der Liste **Projekte** auf **ProductsServer**. Vergewissern Sie sich, dass **ProductsPortal** **nicht** ausgewählt ist.

14. Klicken Sie in der Liste **Projekte** auf **ProductsPortal**. Vergewissern Sie sich, dass **ProductsServer** ausgewählt ist.

    ![][26]

15. Klicken Sie im Dialogfeld **Eigenschaftenseiten** auf **OK**.

## Lokales Ausführen des Projekts

Drücken Sie F5, um die Anwendung in Visual Studio lokal zu testen. Zuerst muss der lokale Server (**ProductsServer**) gestartet werden, gefolgt von der Anwendung **ProductsPortal** in einem Browserfenster. Dieses Mal werden Sie feststellen, dass in der Produktbestandsliste Daten aus dem lokalen Produktdienstsystem aufgeführt werden.

![][10]

Wählen Sie auf der Seite **ProductsPortal** die Option **Aktualisieren** aus. Bei jeder Aktualisierung der Seite wird von der Server-App eine Nachricht angezeigt, wenn `GetProducts()` von **ProductsServer** aufgerufen wird.

Schließen Sie beide Anwendungen, bevor Sie mit dem nächsten Schritt fortfahren.

## Bereitstellen des Projekts ProductsPortal in einer Azure-Web-App

Im nächsten Schritt wird das **ProductsPortal**-Front-End in eine Azure-Web-App konvertiert. Stellen Sie zuerst das Projekt **ProductsPortal** bereit, indem Sie alle Schritte im Abschnitt [Bereitstellen des Webprojekts für die Azure-Web-App](../app-service-web/web-sites-dotnet-get-started.md#deploy-the-web-project-to-the-azure-web-app) ausführen. Kehren Sie nach Abschluss der Bereitstellung zu diesem Tutorial zurück, und fahren Sie mit dem nächsten Schritt fort.

> [AZURE.NOTE] Im Browserfenster wird unter Umständen eine Fehlermeldung angezeigt, wenn das Webprojekt **ProductsPortal** nach der Bereitstellung automatisch gestartet wird. Dies ist zu erwarten und tritt auf, da die Anwendung **ProductsServer** noch nicht ausgeführt wird.

Kopieren Sie die URL der bereitgestellten Web-App, da Sie die URL im nächsten Schritt benötigen. Sie können diese URL auch in Visual Studio im Fenster „Aktivität von Azure App Service“ abrufen:

![][9]

### Festlegen von ProductsPortal als Web-App

Bevor Sie die Anwendung in der Cloud ausführen, müssen Sie dafür sorgen, dass **ProductsPortal** in Visual Studio als Web-App gestartet wird.

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt **ProjectsPortal**, und klicken Sie dann auf **Eigenschaften**.

3. Klicken Sie in der linken Spalte auf **Web**.

5. Klicken Sie im Abschnitt **Aktion starten** auf die Schaltfläche **Start-URL**, und geben Sie im Textfeld die URL für die zuvor bereitgestellte Web-App ein (beispielsweise `http://productsportal1234567890.azurewebsites.net/`).

	![][27]

6. Klicken Sie im Menü **Datei** in Visual Studio auf **Save All**.

7. Klicken Sie in Visual Studio im Menü „Build“ auf **Projektmappe neu erstellen**.

## Ausführen der Anwendung

2.  Drücken Sie F5, um die Anwendung zu erstellen und auszuführen. Zuerst muss der lokale Server (die Konsolenanwendung **ProductsServer**) gestartet werden, gefolgt von der Anwendung **ProductsPortal** in einem Browserfenster, wie im Screenshot weiter unten dargestellt. Beachten Sie auch hier wieder, dass im Produktbestand die aus dem lokalen Produktdienstsystem abgerufenen Daten aufgelistet und in der Web-App angezeigt werden. Vergewissern Sie sich anhand der URL, dass **ProductsPortal** in der Cloud als Azure-Web-App ausgeführt wird.

    ![][1]

	> [AZURE.IMPORTANT] Die Konsolenanwendung **ProductsServer** muss ausgeführt werden, und sie muss die Daten für die Anwendung **ProductsPortal** bereitstellen können. Sollte im Browser ein Fehler angezeigt werden, warten Sie noch einige Sekunden, bis **ProductsServer** geladen wurde und die folgende Nachricht angezeigt wird. Wählen Sie im Browser dann die Option **Aktualisieren** aus.

	![][37]

3. Wählen Sie im Browser auf der Seite **ProductsPortal** die Option **Aktualisieren** aus. Bei jeder Aktualisierung der Seite wird von der Server-App eine Nachricht angezeigt, wenn `GetProducts()` von **ProductsServer** aufgerufen wird.

	![][38]

## Nächste Schritte  

Weitere Informationen zum Servicebus finden Sie in den folgenden Ressourcen:

* [Azure Service Bus][sbwacom]
* [Verwenden von Servicebus-Warteschlangen][sbwacomqhowto]


  [0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
  [1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [Tools und SDK herunterladen]: http://go.microsoft.com/fwlink/?LinkId=271920
  [NuGet]: http://nuget.org
  
  [11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-1.png
  [13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-13.png
  [15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
  [16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-4.png
  [17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-7.png
  [18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-5.png
  [19]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-6.png
  [9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-9.png
  [10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App3.png

  [21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App1.png
  [24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-12.png
  [25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
  [26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
  [27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-8.png
  
  [36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
  [38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
  [41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
  [43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png


  [sbwacom]: /documentation/services/service-bus/
  [sbwacomqhowto]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

<!---HONumber=AcomDC_0928_2016-->