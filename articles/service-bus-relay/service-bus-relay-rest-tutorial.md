---
title: REST-Tutorial zu Service Bus Relay-Messaging | Microsoft Docs
description: Erstellen einer einfachen Service Bus Relay-Hostanwendung, die eine REST-basierte Schnittstelle bereitstellt.
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 1312b2db-94c4-4a48-b815-c5deb5b77a6a
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/27/2016
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 7ba69a1a5f363fe5034e3fc7946b1584c9d77b50


---
# <a name="service-bus-wcf-relay-rest-tutorial"></a>REST-Tutorial zu Service Bus WCF-Relay
Dieses Lernprogramm beschreibt, wie Sie eine einfache Service Bus-Hostanwendung erstellen, die eine REST-basierte Schnittstelle verfügbar macht. REST ermöglicht einem Webclient, z. B. einem Webbrowser, den Zugriff auf die Service Bus-API über HTTP-Anforderungen.

Dieses Lernprogramm verwendet das WCF-REST-Programmiermodell (Windows Communication Foundation) zum Erstellen eines REST-Diensts für Service Bus. Weitere Informationen finden Sie unter [WCF-REST-Programmiermodell](https://msdn.microsoft.com/library/bb412169.aspx) und [Entwerfen und Implementieren von Diensten](https://msdn.microsoft.com/library/ms729746.aspx) in der WCF-Dokumentation.

## <a name="step-1-create-a-service-namespace"></a>Schritt 1: Erstellen eines Dienstnamespace
Der erste Schritt umfasst die Einrichtung eines Namespace und das Abrufen eines Shared Access Signature(SAS)-Schlüssels. Ein Namespace stellt eine Anwendungsgrenze für jede Anwendung bereit, die über Service Bus zur Verfügung steht. Das System generiert automatisch einen SAS-Schlüssel, wenn ein Dienstnamespace erstellt wird. Dienstnamespace und SAS-Schlüssel bilden gemeinsam die Anmeldeinformationen, mit denen sich der Service Bus bei der Anwendung authentifiziert.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="step-2-define-a-rest-based-wcf-service-contract-to-use-with-service-bus"></a>Schritt 2: Definieren eines REST-basierten WCF-Dienstvertrags für die Verwendung mit Service Bus
Wie bei anderen Service Bus-Diensten müssen Sie den Vertrag definieren, wenn Sie einen Dienst im REST-Stil erstellen. Der Vertrag gibt an, welche Vorgänge der Host unterstützt. Ein Dienstvorgang ähnelt einer Webdienstmethode. Verträge werden durch die Definition einer C++-, C#- oder Visual Basic-Schnittstelle erstellt. Jede Methode in der Schnittstelle entspricht einem bestimmten Dienstvorgang. Auf jede Schnittstelle muss das Attribut [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) und auf jeden Vorgang das Attribut [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) angewendet werden. Wenn eine Methode in einer Schnittstelle mit dem Attribut [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) kein Attribut [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) besitzt, wird diese Methode nicht bereitgestellt. Der für diese Aufgaben verwendete Code wird im Beispiel nach dem Verfahren bereitgestellt.

Der Hauptunterschied zwischen einem grundlegenden Service Bus-Vertrag und einem Vertrag im REST-Stil ist das Hinzufügen einer Eigenschaft zum Attribut [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx). Mit dieser Eigenschaft können Sie eine Methode in der Schnittstelle einer Methode auf der anderen Seite der Schnittstelle zuordnen. In diesem Fall verwenden wir [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx), um eine Methode mit HTTP GET zu verknüpfen. Dadurch kann Service Bus Befehlen, die an die Schnittstelle gesendet werden, präzise abrufen und interpretieren.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>So erstellen Sie einen Service Bus-Vertrag mit einer Schnittstelle
1. Öffnen Sie Visual Studio als Administrator: Klicken Sie mit der rechten Maustaste im **Startmenü** auf das Programm, und klicken Sie dann auf **Als Administrator ausführen**.
2. Erstellen Sie ein neues Konsolenanwendungsprojekt. Klicken Sie auf das Menü **Datei**, wählen Sie **Neu** und dann **Projekt** aus. Klicken Sie im Dialogfeld **Neues Projekt** auf **Visual C#**, wählen Sie die Vorlage **Konsolenanwendung** aus, und nennen Sie sie **ImageListener**. Verwenden Sie den standardmäßigen **Speicherort**. Klicken Sie auf **OK** , um das Projekt zu erstellen.
3. Für ein C#-Projekt erstellt Visual Studio erstellt eine `Program.cs`-Datei. Diese Klasse enthält eine leere `Main()`-Methode, die erforderlich ist, damit ein Konsolenanwendungsprojekt ordnungsgemäß erstellt wird.
4. Fügen Sie dem Projekt Verweise auf Service Bus und **System.ServiceModel.dll** hinzu, indem Sie das NuGet-Paket „Service Bus“ installieren. Dieses Paket fügt automatisch Verweise auf die Service Bus-Bibliotheken sowie **System.ServiceModel** (WCF) hinzu. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **ImageListener**, und klicken Sie anschließend auf **NuGet-Pakete verwalten**. Klicken Sie auf die Registerkarte **Durchsuchen**, und suchen Sie nach `Microsoft Azure Service Bus`. Klicken Sie auf **Installieren**, und akzeptieren Sie die Nutzungsbedingungen.
5. Dem Projekt muss explizit ein Verweis auf **System.ServiceModel.Web.dll** hinzugefügt werden:
   
    a. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste unter dem Projektordner auf den Ordner **Verweise**, und klicken Sie dann auf **Verweis hinzufügen**.
   
    b. Klicken Sie links im Dialogfeld **Verweis hinzufügen** auf die Registerkarte **Framework**, und geben Sie im **Suchfeld** die Zeichenfolge **System.ServiceModel.Web** ein. Aktivieren Sie das Kontrollkästchen **System.ServiceModel.Web**, und klicken Sie anschließend auf **OK**.
6. Fügen Sie am Anfang der Datei „Program.cs“ die folgenden `using`-Anweisungen hinzu:
   
    ```
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```
   
    [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) ist der Namespace, der den programmgesteuerten Zugriff auf die grundlegenden Funktionen von WCF ermöglicht. Service Bus verwendet viele Objekte und Attribute von WCF, um Dienstverträge zu definieren. Sie verwenden diesen Namespace in den meisten Ihrer Service Bus Relay-Anwendungen. Auf ähnliche Weise trägt [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) zur Definition des Kanals bei, der das Objekt darstellt, über das Sie mit Service Bus und dem Clientwebbrowser kommunizieren. Schließlich enthält [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) die Typen, mit denen Sie webbasierte Anwendungen erstellen können.
7. Versehen Sie den `ImageListener`-Namespace mit dem Namen **Microsoft.ServiceBus.Samples**.
   
     ```
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```
8. Definieren Sie direkt nach der öffnenden geschweiften Klammer der Namespacedeklaration eine neue Schnittstelle namens **IImageContract**, und wenden Sie das Attribut **ServiceContractAttribute** mit dem Wert `http://samples.microsoft.com/ServiceModel/Relay/` auf die Schnittstelle an. Der Namespacewert unterscheidet sich von dem Namespace, den Sie im gesamten Codebereich verwenden. Der Namespacewert dient als eindeutiger Bezeichner für diesen Vertrag und sollte über Versionsinformationen verfügen. Weitere Informationen finden Sie unter [Dienstversionsverwaltung](http://go.microsoft.com/fwlink/?LinkID=180498). Das explizite Angeben des Namespace verhindert, dass der Standardwert für den Namespace dem Vertragsnamen hinzugefügt wird.
   
    ```
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```
9. Deklarieren Sie innerhalb der `IImageContract`-Benutzeroberfläche eine Methode für den einzelnen Vorgang, den der `IImageContract`-Vertrag in der Schnittstelle bereitstellt, und wenden Sie das `OperationContractAttribute`-Attribut auf die Methode an, die Sie als Teil des öffentlichen Service Bus-Vertrags bereitstellen möchten.
   
    ```
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```
10. Fügen Sie im **OperationContract**-Attribut den Wert **WebGet** hinzu.
    
    ```
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```
    
    Auf diese Weise kann Service Bus HTTP GET-Anforderungen an `GetImage` weiterleiten und die Rückgabewerte von `GetImage` in eine HTTP GETRESPONSE-Antwort umwandeln. Später in diesem Lernprogramm verwenden Sie einen Webbrowser für den Zugriff auf diese Methode und zum Anzeigen des Bilds im Browser.
11. Direkt nach der `IImageContract`-Definition deklarieren Sie einen Kanal, der von den beiden `IImageContract`- und `IClientChannel`-Schnittstellen erbt.
    
    ```
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```
    
    Ein Kanal ist das WCF-Objekt, über das der Dienst und der Client gegenseitig Informationen übergeben. Später erstellen Sie den Kanal in Ihrer Hostanwendung. Service Bus verwendet dann diesen Kanal, um die HTTP GET-Anforderungen vom Browser an Ihre **GetImage**-Implementierung zu übergeben. Service Bus verwendet den Kanal auch, um den **GetImage**-Rückgabewert in eine HTTP GETRESPONSE für den Clientbrowser zu übersetzen.
12. Klicken Sie im Menü **Build** auf **Projektmappe erstellen**, um die Richtigkeit Ihrer bisherigen Arbeit zu überprüfen.

### <a name="example"></a>Beispiel
Das folgende Codebeispiel zeigt eine Basisschnittstelle, die einen Service Bus-Vertrag definiert.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="step-3-implement-a-rest-based-wcf-service-contract-to-use-service-bus"></a>Schritt 3: Implementieren eines REST-basierten WCF-Dienstvertrags für die Verwendung von Service Bus
Das Erstellen eines Service Bus-Diensts im REST-Stil erfordert, dass Sie zuerst den Vertrag erstellen, der über eine Schnittstelle definiert wird. Der nächste Schritt ist das Implementieren der Schnittstelle. Dies umfasst das Erstellen einer Klasse namens **ImageService**, die die benutzerdefinierte **IImageContract**-Schnittstelle implementiert. Nachdem Sie den Vertrag implementiert haben, konfigurieren Sie die Schnittstelle mithilfe einer "App.config"-Datei. Die Konfigurationsdatei enthält die erforderlichen Informationen für die Anwendung, wie z. B. den Namen des Diensts, den Namen des Vertrags und den Typ des Protokolls, das für die Kommunikation mit Service Bus verwendet wird. Der für diese Aufgaben verwendete Code wird im Beispiel nach dem Verfahren bereitgestellt.

Wie bei den vorherigen Schritten gibt es nur wenige Unterschiede zwischen der Implementierung eines Vertrags im REST-Stil und eines grundlegenden Service Bus-Vertrags.

### <a name="to-implement-a-rest-style-service-bus-contract"></a>So implementieren Sie einen Service Bus-Vertrag im REST-Stil
1. Erstellen Sie direkt nach der Definition der **IImageContract**-Schnittstelle eine neue Klasse namens **ImageService**. Die **ImageService**-Klasse implementiert die **IImageContract**-Schnittstelle.
   
    ```
    class ImageService : IImageContract
    {
    }
    ```
    Ähnlich wie bei anderen Schnittstellenimplementierungen können Sie die Definition in einer anderen Datei implementieren. In diesem Tutorial findet die Implementierung allerdings in derselben Datei wie die Schnittstellendefinition und die `Main()`-Methode statt.
2. Wenden Sie das Attribut [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) auf die **IImageService**-Klasse an, um anzugeben, dass die Klasse eine Implementierung eines WCF-Vertrags ist.
   
    ```
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```
   
    Wie bereits erwähnt, ist dieser Namespace kein herkömmlicher Namespace. Stattdessen ist es Teil der WCF-Architektur, die den Vertrag identifiziert. Weitere Informationen finden Sie im Thema [Datenvertragsnamen](https://msdn.microsoft.com/library/ms731045.aspx) in der WCF-Dokumentation.
3. Fügen Sie dem Projekt ein JPG-Bild hinzu.  
   
    Dies ist ein Bild, das der Dienst im empfangenden Browser anzeigt. Klicken Sie mit der rechten Maustaste auf das Projekt, und klicken Sie dann auf **Hinzufügen**. Klicken Sie anschließend auf **Vorhandenes Element**. Verwenden Sie das Dialogfeld **Vorhandenes Element hinzufügen**, um zu einer geeigneten JPG-Datei zu navigieren, und klicken Sie auf **Hinzufügen**.
   
    Stellen Sie beim Hinzufügen der Datei sicher, dass in der Dropdownliste neben dem Feld **Dateiname** die Option **Alle Dateien** ausgewählt ist. Im Rest dieses Lernprogramms wird davon ausgegangen, dass der Name des Bilds "image.jpg" lautet. Wenn Sie eine andere Datei haben, müssen Sie das Bild umbenennen oder den Code entsprechend ändern.
4. Stellen Sie sicher, dass der ausgeführte Dienst die Bilddatei finden kann. Klicken Sie hierzu im **Projektmappen-Explorer** mit der rechten Maustaste auf die Bilddatei, und klicken Sie anschließend auf **Eigenschaften**. Legen Sie im Bereich **Eigenschaften** den Wert für **In Ausgabeverzeichnis kopieren** auf **Kopieren, wenn neuer** fest.
5. Fügen Sie dem Projekt einen Verweis auf die Assembly **System.Drawing.dll`using` sowie die folgenden dazugehörigen **-Anweisungen hinzu.  
   
    ```
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```
6. Fügen Sie in der **ImageService**-Klasse den folgenden Konstruktor hinzu, der die Bitmap lädt und das Senden an den Clientbrowser vorbereitet.
   
    ```
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";
   
        Image bitmap;
   
        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```
7. Fügen Sie direkt nach dem vorherigen Code die folgende Methode **GetImage** in der **ImageService**-Klasse hinzu, um eine HTTP-Nachricht mit dem Bild zurückzugeben.
   
    ```
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);
   
        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";
   
        return stream;
    }
    ```
   
    Diese Implementierung verwendet **MemoryStream** zum Abrufen des Bilds sowie zum Vorbereiten des Streamings an den Browser. Sie startet die Streamposition bei null, deklariert den Streaminhalt als JPEG und streamt dann die Informationen.
8. Klicken Sie im Menü **Build** auf **Projektmappe erstellen**.

### <a name="to-define-the-configuration-for-running-the-web-service-on-service-bus"></a>So definieren Sie die Konfiguration zum Ausführen des Webdiensts auf Service Bus
1. Doppelklicken Sie im **Projektmappen-Explorer** auf **App.config**, um die Datei im Visual Studio-Editor zu öffnen.
   
    Die Datei **App.config** gleicht einer WCF-Konfigurationsdatei und enthält den Dienstnamen, den Endpunkt (also den Ort, den Service Bus für die Kommunikation zwischen Clients und Hosts bereitstellt) und die Bindung (die Art des Protokolls für die Kommunikation). Der Hauptunterschied besteht hier darin, dass der konfigurierte Dienstendpunkt auf eine [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx)-Bindung verweist, die nicht Teil von .NET Framework ist.
2. Das XML-Element `<system.serviceModel>` ist ein WCF-Element und definiert mindestens einen Dienst. Hier wird es verwendet, um den Dienstnamen und den Endpunkt zu definieren. Fügen Sie am Ende des `<system.serviceModel>`-Elements (aber immer noch innerhalb von `<system.serviceModel>`) ein `<bindings>`-Element mit dem weiter unten angegebenen Inhalt hinzu. Dies definiert die in der Anwendung verwendeten Bindungen. Sie können mehrere Bindungen definieren, aber in diesem Lernprogramm definieren Sie nur eine.
   
    ```
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```
   
    Dieser Schritt definiert eine Service Bus-[WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx)-Bindung mit auf **None** festgelegtem **relayClientAuthenticationType**. Diese Einstellung gibt an, dass ein Endpunkt mit dieser Bindung keine Clientanmeldeinformationen erfordert.
3. Fügen Sie nach dem `<bindings>`-Element ein `<services>`-Element hinzu. Ähnlich wie Bindungen können Sie mehrere Dienste in einer einzigen Konfigurationsdatei definieren. Für dieses Lernprogramm definieren Sie aber nur einen.
   
    ```
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```
   
    Dieser Schritt konfiguriert einen Dienst, der die zuvor definierte standardmäßige **webHttpRelayBinding**-Bindung verwendet. Darüber hinaus verwendet er den standardmäßigen **sbTokenProvider**-Anbieter, der im nächsten Schritt definiert wird.
4. Erstellen Sie nach dem `<services>`-Element ein `<behaviors>`-Element mit dem folgenden Inhalt, das „SAS_KEY“ durch den *Shared Access Signature*-Schlüssel (SAS) ersetzt, den Sie zuvor aus dem [Azure-Portal][Azure-Portal] abgerufen haben.
   
    ```
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```
5. Ersetzen Sie in „App.config“ im `<appSettings>`-Element den gesamten Wert der Verbindungszeichenfolge durch die Verbindungszeichenfolge, die Sie zuvor aus dem Portal abgerufen haben. 
   
    ```
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```
6. Klicken Sie im Menü **Build** auf **Projektmappe erstellen**, um die gesamte Projektmappe zu erstellen.

### <a name="example"></a>Beispiel
Der folgende Code zeigt die Vertrags- und Dienstimplementierung für einen REST-basierten Dienst, der mit der **WebHttpRelayBinding**-Bindung auf dem Service Bus ausgeführt wird.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Das folgende Beispiel zeigt die "App.config"-Datei, die dem Dienst zugeordnet ist.

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove the ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="[SAS_KEY]" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app setings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
</configuration>
```

## <a name="step-4-host-the-rest-based-wcf-service-to-use-service-bus"></a>Schritt 4: Hosten des REST-basierten WCF-Diensts zur Verwendung von Service Bus
Dieser Schritt beschreibt, wie ein Webdienst mithilfe einer Konsolenanwendung auf Service Bus ausgeführt wird. Eine vollständige Liste des Programmcodes, der in diesem Schritt geschrieben wird, wird in dem Beispiel nach dem Verfahren bereitgestellt.

### <a name="to-create-a-base-address-for-the-service"></a>So erstellen Sie eine Basisadresse für den Dienst
1. Erstellen Sie in der `Main()`-Funktionsdeklaration eine Variable zum Speichern des Namespace Ihres Service Bus-Projekts. Ersetzen Sie `yourNamespace` durch den Namen des zuvor erstellten Dienstnamespace.
   
    ```
    string serviceNamespace = "yourNamespace";
    ```
    Service Bus verwendet den Namespace-Namen, um einen eindeutigen URI zu erstellen.
2. Erstellen Sie eine `Uri`-Instanz für die Basisadresse des Diensts, der auf dem Namespace basiert.
   
    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="to-create-and-configure-the-web-service-host"></a>So erstellen und konfigurieren Sie den Webdiensthost
* Erstellen Sie den Webdiensthost mithilfe der URI-Adresse, die Sie weiter oben in diesem Abschnitt erstellt haben.
  
    ```
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    Der Diensthost ist das WCF-Objekt, das die Hostanwendung instanziiert. In diesem Beispiel übergeben Sie den Typ des Hosts, den Sie erstellen möchten (**ImageService**), sowie die Adresse, an der Sie die Hostanwendung bereitstellen möchten.

### <a name="to-run-the-web-service-host"></a>So führen Sie den Webdiensthost aus
1. Öffnen Sie den Dienst.
   
    ```
    host.Open();
    ```
    Der Dienst wird jetzt ausgeführt.
2. Zeigen Sie eine Meldung an, die angibt, dass der Dienst ausgeführt wird und wie er beendet wird.
   
    ```
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```
3. Schließen Sie danach den Diensthost.
   
    ```
    host.Close();
    ```

## <a name="example"></a>Beispiel
Das folgende Beispiel schließt den Dienstvertrag und die Implementierung aus früheren Schritten des Lernprogramms ein und hostet den Dienst in einer Konsolenanwendung. Kompilieren Sie den folgenden Code in eine ausführbare Datei namens „ImageListener.exe“.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy the following address into a browser to see the image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### <a name="compiling-the-code"></a>Kompilieren des Codes
Führen Sie nach dem Erstellen der Projektmappe Folgendes aus, um die Anwendung auszuführen:

1. Drücken Sie **F5**, oder navigieren Sie zum Speicherort der ausführbaren Datei (ImageListener\bin\Debug\ImageListener.exe), um den Dienst auszuführen. Führen Sie die App weiter aus, da sie für den nächsten Schritt erforderlich ist.
2. Kopieren Sie die Adresse aus der Eingabeaufforderung in einen Browser, um das Bild anzuzeigen.
3. Drücken Sie schließlich im Eingabeaufforderungsfenster die **EINGABETASTE**, um die App zu schließen.

## <a name="next-steps"></a>Nächste Schritte
Da Sie jetzt eine Anwendung erstellt haben, die den Service Bus Relay-Dienst verwendet, finden Sie in den folgenden Artikeln weitere Informationen zum Relaymessaging:

* [Übersicht über die Architektur von Azure Service Bus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)
* [Verwenden des Azure Service Bus WCF-Relay-Diensts](service-bus-dotnet-how-to-use-relay.md)

[Azure-Portal]: https://portal.azure.com



<!--HONumber=Nov16_HO3-->


