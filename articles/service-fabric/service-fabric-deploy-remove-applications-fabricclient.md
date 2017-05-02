---
title: Azure Service Fabric-Anwendungsbereitstellung | Microsoft-Dokumentation
description: Bereitstellen und Entfernen von Anwendungen in Service Fabric mithilfe der FabricClient-APIs
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b120ffbf-f1e3-4b26-a492-347c29f8f66b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/10/2017
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: cc9e81de9bf8a3312da834502fa6ca25e2b5834a
ms.openlocfilehash: 4c7a92352d4fef6a0e55b608bf5c957cd2c40332
ms.lasthandoff: 04/11/2017


---
# <a name="deploy-and-remove-applications-using-fabricclient"></a>Bereitstellen und Entfernen von Anwendungen mithilfe von FabricClient
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Visual Studio](service-fabric-publish-app-remote-cluster.md)
> * [FabricClient-APIs](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

Sobald der [Anwendungstyp gepackt][10] wurde, ist die Anwendung für die Bereitstellung in einem Azure Service Fabric-Cluster bereit. Die Bereitstellung umfasst die folgenden drei Schritte:

1. Hochladen des Anwendungspakets in den Imagespeicher
2. Registrierung des Anwendungstyps
3. Erstellen der Anwendungsinstanz

Nachdem eine App bereitgestellt wurde und eine Instanz im Cluster ausgeführt wird, können Sie die App-Instanz und ihren Anwendungstyp löschen. Das vollständige Entfernen einer App aus dem Cluster umfasst die folgenden Schritte:

1. Entfernen (oder Löschen) der ausgeführten Anwendungsinstanz
2. Aufheben der Registrierung des Anwendungstyps, wenn er nicht mehr benötigt wird
3. Entfernen des Anwendungspakets aus dem Image-Speicher

Wenn Sie [Visual Studio zum Bereitstellen und Debuggen von Anwendungen](service-fabric-publish-app-remote-cluster.md) in Ihrem lokalen Entwicklungscluster verwenden, werden alle vorherigen Schritte automatisch über ein PowerShell-Skript ausgeführt.  Dieses Skript befindet sich im Ordner *Skripts* des Anwendungsprojekts. In diesem Artikel wird die grundlegende Funktionsweise dieses Skripts erläutert, sodass Sie die gleichen Vorgänge außerhalb von Visual Studio ausführen können. 
 
## <a name="connect-to-the-cluster"></a>Verbinden mit dem Cluster
Stellen Sie durch Erstellen einer [FabricClient](/dotnet/api/system.fabric.fabricclient)-Instanz eine Verbindung mit dem Cluster her, bevor Sie die Codebeispiele in diesem Artikel ausführen. Beispiele für das Herstellen einer Verbindung mit einem lokalen Entwicklungscluster oder einem Remotecluster bzw. einem mit Azure Active Directory, X509-Zertifikaten oder Windows Active Directory gesicherten Cluster finden Sie unter [Herstellen einer Verbindung mit einem sicheren Cluster](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-the-fabricclient-apis).  Führen Sie zum Herstellen der Verbindung mit dem lokalen Entwicklungscluster den folgenden Befehl aus:

```csharp
// Connect to the local cluster.
FabricClient fabricClient = new FabricClient();
```

## <a name="upload-the-application-package"></a>Hochladen des Anwendungspakets
Angenommen, Sie erstellen und verpacken eine App namens *MyApplication* in Visual Studio. Der Name des Anwendungstyps ist in der Datei „ApplicationManifest.xml“ standardmäßig als „MyApplicationType“ aufgeführt.  Das Anwendungspaket mit den erforderlichen Anwendungs- und Dienstmanifesten sowie Code-/Konfigurations-/Datenpaketen befindet sich in *C:\Benutzer\Benutzername\Eigene Dokumente\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug*.

Beim Hochladen des Anwendungspakets wird das Paket an einem Speicherort gespeichert, an dem interne Service Fabric-Komponenten auf das Paket zugreifen können.
Wenn Sie das Anwendungspaket lokal überprüfen möchten, verwenden Sie das Cmdlet [Test-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage).

Mit der [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage)-Methode wird das Anwendungspaket in den Clusterimagespeicher hochgeladen. 

Wenn das Anwendungspaket groß ist und/oder viele Dateien enthält, können Sie es [komprimieren](service-fabric-package-apps.md#compress-a-package) und unter Verwendung von PowerShell in den Imagespeicher kopieren. Durch eine Komprimierung werden Größe und Anzahl der Dateien verringert.

Weitere Informationen zum Imagespeicher und zur Imagespeicher-Verbindungszeichenfolge finden Sie unter [Grundlegendes zur Imagespeicher-Verbindungszeichenfolge](service-fabric-image-store-connection-string.md).

## <a name="register-the-application-package"></a>Registrieren des Anwendungspakets
Der Anwendungstyp und die Version, der bzw. die im Anwendungsmanifest deklariert sind, werden beim Registrieren des Anwendungspakets verfügbar. Das System liest das im vorherigen Schritt hochgeladene Paket, überprüft es, verarbeitet den Inhalt des Pakets und kopiert das verarbeitete Paket an einen internen Systemspeicherort.  

Mit der [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync)-Methode wird der Anwendungstyp im Cluster registriert und für die Bereitstellung verfügbar gemacht.

Mit der [GetApplicationTypeListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationtypelistasync)-Methode werden alle erfolgreich registrierten Anwendungstypversionen und deren Registrierungsstatus aufgelistet. Sie können mithilfe dieses Befehls ermitteln, wann die Registrierung abgeschlossen ist.

## <a name="create-an-application-instance"></a>Erstellen einer Anwendungsinstanz
Sie können eine Anwendung mit einer beliebigen Version des Anwendungstyps instanziieren, die mit der [CreateApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync)-Methode erfolgreich registriert wurde. Der Name jeder Anwendung muss mit dem *fabric:* -Schema beginnen und für jede Anwendungsinstanz eindeutig sein. Wenn im Anwendungsmanifest des Zielanwendungstyps Standarddienste festgelegt wurden, werden diese ebenfalls erstellt.

Für jede Version des registrierten Anwendungstyps können mehrere Anwendungsinstanzen erstellt werden. Jede Anwendungsinstanz wird isoliert mit einem eigenen Arbeitsverzeichnis und Prozess ausgeführt.

Um anzuzeigen, welche benannten Apps und Dienste im Cluster ausgeführt werden, führen Sie die Methoden [GetApplicationListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync) und [GetServiceListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync) aus.

## <a name="create-a-service-instance"></a>Erstellen einer Dienstinstanz
Mit der [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync)-Methode können Sie einen Dienst über einen Diensttyp instanziieren.  Wenn der Dienst im Anwendungsmanifest als Standarddienst deklariert ist, wird er beim Instanziieren der Anwendung instanziiert.  Wenn die [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync)-Methode für einen Dienst aufgerufen wird, der bereits instanziiert ist, wird eine Ausnahme zurückgegeben. 

## <a name="remove-a-service-instance"></a>Entfernen einer Dienstinstanz
Wenn eine Dienstinstanz nicht mehr benötigt wird, können Sie sie durch Aufrufen der [DeleteServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync)-Methode aus der ausgeführten Anwendungsinstanz entfernen.  Dieser Vorgang kann nicht rückgängig gemacht werden, und der Dienststatus kann nicht wiederhergestellt werden.

## <a name="remove-an-application-instance"></a>Entfernen einer Anwendungsinstanz
Wenn eine Anwendungsinstanz nicht mehr benötigt wird, können Sie sie anhand des Namens mit der [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync)-Methode endgültig entfernen. Mit [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) werden auch alle Dienste automatisch entfernt, die mit der Anwendung verknüpft sind, d.h., der Dienststatus wird vollständig und dauerhaft entfernt. Dieser Vorgang kann nicht rückgängig gemacht werden, und der Anwendungsstatus kann nicht wiederhergestellt werden.

## <a name="unregister-an-application-type"></a>Aufheben der Registrierung eines Anwendungstyps
Wenn eine bestimmte Version eines Anwendungstyps nicht mehr benötigt wird, sollten Sie die Registrierung des Anwendungstyps mit der [Unregister-ServiceFabricApplicationType](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync)-Methode aufheben. Durch das Aufheben der Registrierung nicht verwendeter Anwendungstypen wird Speicherplatz freigegeben, der vom Imagespeicher verwendet wird. Die Registrierung eines Anwendungstyps kann nur aufgehoben werden, wenn keine Anwendungen für den Typ instanziiert sind und keine ausstehenden Anwendungsupgrades vorliegen, die auf den Typ verweisen.

## <a name="remove-an-application-package-from-the-image-store"></a>Entfernen eines Anwendungspakets aus dem Imagespeicher
Wenn ein Anwendungspaket nicht mehr benötigt wird, können Sie es mit der [RemoveApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage)-Methode aus dem Imagespeicher löschen, um Systemressourcen freizugeben.

## <a name="troubleshooting"></a>Problembehandlung
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Copy-ServiceFabricApplicationPackage fordert einen ImageStoreConnectionString an
Die Service Fabric-SDK-Umgebung sollte bereits mit den richtigen Standardeinstellungen eingerichtet sein. Wichtig ist, dass der ImageStoreConnectionString für alle Befehle mit dem vom Service Fabric-Cluster verwendeten Wert übereinstimmt. Sie finden die Imagespeicher-Verbindungszeichenfolge im Clustermanifest, das mit der [Get-ServiceFabricClusterManifest](/dotnet/api/system.fabric.fabricclient.clustermanagementclient.getclustermanifestasync)-Methode abgerufen werden kann.

ImageStoreConnectionString ist im Clustermanifest zu finden:

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

Weitere Informationen zum Imagespeicher und zur Imagespeicher-Verbindungszeichenfolge finden Sie unter [Grundlegendes zur Imagespeicher-Verbindungszeichenfolge](service-fabric-image-store-connection-string.md).

### <a name="deploy-large-application-package"></a>Bereitstellen eines großen Anwendungspakets
Problem: Bei einem großen Anwendungspaket (im GB-Bereich) tritt bei Verwendung der [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage)-Methode ein Timeout auf.
Versuchen Sie Folgendes:
- Geben Sie für die [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage)-Methode mit dem Parameter `timeout` einen höheren Timeoutwert an. Das Timeout beträgt standardmäßig 30 Minuten.
- Überprüfen Sie die Netzwerkverbindung zwischen Ihrem Quellcomputer und dem Cluster. Ziehen Sie bei einer langsamen Verbindung die Verwendung eines Computers mit einer besseren Netzwerkverbindung in Betracht.
Wenn sich der Clientcomputer in einem anderen Bereich als der Cluster befindet, sollten Sie einen Clientcomputer in einem näher gelegenen oder im selben Bereich wie den des Clusters verwenden.
- Überprüfen Sie, ob Sie von einer externen Drosselung betroffen sind. Wenn der Abbildspeicher beispielsweise für die Verwendung des Azure-Speichers konfiguriert ist, wird der Upload eventuell gedrosselt.

Problem: Das Paket wurde erfolgreich hochgeladen, allerdings tritt bei Verwendung der [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync)-Methode ein Timeout auf.
Versuchen Sie Folgendes:
- [Komprimieren Sie das Paket](service-fabric-package-apps.md#compress-a-package), bevor Sie es in den Abbildspeicher kopieren.
Durch eine Komprimierung werden Größe und Anzahl der Dateien verringert, wodurch wiederum die Menge des Datenverkehrs und der Aufgaben, die das Service Fabric durchführen muss, reduziert wird. Der Upload kann sich eventuell verlangsamen (insbesondere, wenn Sie die Komprimierungszeit berücksichtigen), allerdings werden die Registrierung und die Aufhebung der Registrierung des Anwendungstyps schneller durchgeführt.
- Geben Sie für die [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync)-Methode mit dem Parameter `timeout` einen höheren Timeoutwert an.

### <a name="deploy-application-package-with-many-files"></a>Bereitstellen eines Anwendungspakets mit vielen Dateien
Problem: Bei einem Anwendungspaket mit vielen Dateien (im Tausenderbereich) tritt bei Verwendung der [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync)-Methode ein Timeout auf.
Versuchen Sie Folgendes:
- [Komprimieren Sie das Paket](service-fabric-package-apps.md#compress-a-package), bevor Sie es in den Abbildspeicher kopieren. Durch eine Komprimierung wird die Anzahl der Dateien verringert.
- Geben Sie für [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) mit dem Parameter `timeout` einen höheren Timeoutwert an.

## <a name="code-example"></a>Codebeispiel
Im folgenden Beispiel wird ein Anwendungspaket in den Imagespeicher kopiert, der Anwendungstyp bereitgestellt, eine Anwendungsinstanz erstellt, eine Dienstinstanz erstellt, die Anwendungsinstanz entfernt, die Bereitstellung des Anwendungstyps aufgehoben und das Anwendungspaket aus dem Imagespeicher gelöscht.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Reflection;
using System.Threading.Tasks;

using System.Fabric;
using System.Fabric.Description;
using System.Threading;

namespace ServiceFabricAppLifecycle
{
class Program
{
static void Main(string[] args)
{

    string clusterConnection = "localhost:19000";
    string appName = "fabric:/MyApplication";
    string appType = "MyApplicationType";
    string appVersion = "1.0.0";
    string serviceName = "fabric:/MyApplication/Stateless1";
    string imageStoreConnectionString = "file:C:\\SfDevCluster\\Data\\ImageStoreShare";
    string packagePathInImageStore = "MyApplication";
    string packagePath = "C:\Users\username\Documents\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug";
    string serviceType = "Stateless1Type";

    // Connect to the cluster.
    FabricClient fabricClient = new FabricClient(clusterConnection);

    // Copy the application package to a location in the image store
    try
    {
        fabricClient.ApplicationManager.CopyApplicationPackage(imageStoreConnectionString, packagePath, packagePathInImageStore);
        Console.WriteLine("Application package copied to {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Application package copy to Image Store failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Provision the application.  "MyApplicationV1" is the folder in the image store where the app package is located. 
    // The application type with name "MyApplicationType" and version "1.0.0" (both are found in the application manifest) 
    // is now registered in the cluster.            
    try
    {
        fabricClient.ApplicationManager.ProvisionApplicationAsync(packagePathInImageStore).Wait();

        Console.WriteLine("Provisioned application type {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Provision Application Type failed:");

        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    //  Create the application instance.
    try
    {
        ApplicationDescription appDesc = new ApplicationDescription(new Uri(appName), appType, appVersion);
        fabricClient.ApplicationManager.CreateApplicationAsync(appDesc).Wait();
        Console.WriteLine("Created application instance of type {0}, version {1}", appType, appVersion);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("CreateApplication failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Create the stateless service description.  For stateful services, use a StatefulServiceDescription object.
    StatelessServiceDescription serviceDescription = new StatelessServiceDescription();
    serviceDescription.ApplicationName = new Uri(appName);
    serviceDescription.InstanceCount = 1;
    serviceDescription.PartitionSchemeDescription = new SingletonPartitionSchemeDescription();
    serviceDescription.ServiceName = new Uri(serviceName);
    serviceDescription.ServiceTypeName = serviceType;

    // Create the service instance.  If the service is declared as a default service in the ApplicationManifest.xml,
    // the service instance is already running and this call will fail.
    try
    {
        fabricClient.ServiceManager.CreateServiceAsync(serviceDescription).Wait();
        Console.WriteLine("Created service instance {0}", serviceName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("CreateService failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete a service instance.
    try
    {
        DeleteServiceDescription deleteServiceDescription = new DeleteServiceDescription(new Uri(serviceName));

        fabricClient.ServiceManager.DeleteServiceAsync(deleteServiceDescription);
        Console.WriteLine("Deleted service instance {0}", serviceName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("DeleteService failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete an application instance from the application type.
    try
    {
        DeleteApplicationDescription deleteApplicationDescription = new DeleteApplicationDescription(new Uri(appName));

        fabricClient.ApplicationManager.DeleteApplicationAsync(deleteApplicationDescription).Wait();
        Console.WriteLine("Deleted application instance {0}", appName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("DeleteApplication failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Un-provision the application type.
    try
    {
        fabricClient.ApplicationManager.UnprovisionApplicationAsync(appType, appVersion).Wait();
        Console.WriteLine("Un-provisioned application type {0}, version {1}", appType, appVersion);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Un-provision application type failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete the application package from a location in the image store.
    try
    {
        fabricClient.ApplicationManager.RemoveApplicationPackage(imageStoreConnectionString, packagePathInImageStore);
        Console.WriteLine("Application package removed from {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Application package removal from Image Store failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    Console.WriteLine("Hit enter...");
    Console.Read();
}        
}
}

```

## <a name="next-steps"></a>Nächste Schritte
[Service Fabric-Anwendungsupgrade](service-fabric-application-upgrade.md)

[Einführung in Service Fabric-Integrität](service-fabric-health-introduction.md)

[Diagnose und Problembehandlung von Service Fabric-Diensten](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Modellieren einer Anwendung in Service Fabric](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md

