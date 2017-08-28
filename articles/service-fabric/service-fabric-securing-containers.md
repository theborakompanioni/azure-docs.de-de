---
title: Azure Service Fabric-Containersicherheit | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie Containerdienste schützen."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.translationtype: HT
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: a262730aec6ce5a1c6f3b7d2d41056a6e6edfbe0
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---

# <a name="container-security"></a>Containersicherheit

Service Fabric bietet einen Mechanismus, über den Dienste innerhalb eines Containers auf ein Zertifikat zugreifen können, das auf den Knoten eines Windows- oder Linux-Clusters (ab Version 5.7) installiert ist. Für Windows-Container unterstützt Service Fabric zudem auch gruppenverwaltete Dienstkonten (group Managed Service Accounts, gMSAs). 

## <a name="certificate-management-for-containers"></a>Zertifikatverwaltung für Container

Containerdienste können durch Angabe eines Zertifikats geschützt werden. Das Zertifikat muss auf den Knoten des Clusters installiert werden. Die Zertifikatinformationen werden im Anwendungsmanifest unter dem Tag `ContainerHostPolicies` angegeben, wie im folgenden Codeausschnitt zu sehen:

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
    <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[Thumbprint1]"/>
    <CertificateRef Name="MyCert2" X509FindValue="[Thumbprint2]"/>
 ```

Beim Start der Anwendung liest die Laufzeit die Zertifikate und generiert eine PFX-Datei und ein Kennwort für jedes Zertifikat. Auf diese PFX-Datei und das Kennwort kann innerhalb des Containers über folgende Umgebungsvariablen zugegriffen werden: 

* **Certificate_[Name des Codepakets]_[Zertifikatname]_PFX**
* **Certificate_[Name des Codepakets]_[Zertifikatname]_Password**

Die PFX-Datei wird vom Containerdienst oder -prozess in den Container importiert. Für den Zertifikatimport können Sie Skripts vom Typ `setupentrypoint.sh` verwenden oder benutzerdefinierten Code innerhalb des Containers ausführen. C#-Beispielcode zum Importieren der PFX-Datei:

```c#
    string certificateFilePath = Environment.GetEnvironmentVariable("Certificate_NodeContainerService.Code_MyCert1_PFX");
    string passwordFilePath = Environment.GetEnvironmentVariable("Certificate_NodeContainerService.Code_MyCert1_Password");
    X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
    string password = File.ReadAllLines(passwordFilePath, Encoding.Default)[0];
    password = password.Replace("\0", string.Empty);
    X509Certificate2 cert = new X509Certificate2(certificateFilePath, password, X509KeyStorageFlags.MachineKeySet | X509KeyStorageFlags.PersistKeySet);
    store.Open(OpenFlags.ReadWrite);
    store.Add(cert);
    store.Close();
```
Dieses PFX-Zertifikat kann verwendet werden, um die Anwendung oder den Dienst zu authentifizieren oder um die Kommunikation mit anderen Diensten zu schützen.


## <a name="set-up-gmsa-for-windows-containers"></a>Einrichten von gMSA für Windows-Container

Zur Einrichtung gruppenverwalteter Dienstkonten (group Managed Service Accounts, gMSAs) wird eine Datei mit Anmeldeinformationen (`credspec`) auf allen Knoten im Cluster platziert. Die Datei kann mithilfe einer VM-Erweiterung auf alle Knoten kopiert werden.  Die Datei vom Typ `credspec` muss die gMSA-Kontoinformationen enthalten. Weitere Informationen zur Datei vom Typ `credspec` finden Sie unter [Service Accounts](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts) (Dienstkonten). Die Angabe von Anmeldeinformationen und das Tag `Hostname` werden im Anwendungsmanifest angegeben. Das Tag `Hostname` muss dem gMSA-Kontonamen entsprechen, unter dem der Container ausgeführt wird.  Durch das Tag `Hostname` kann sich der Container gegenüber anderen Diensten in der Domäne mittels Kerberos-Authentifizierung authentifizieren.  Der folgende Codeausschnitt zeigt ein Beispiel für die Angabe von `Hostname` und `credspec` im Anwendungsmanifest:

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen eines Windows-Containers in Service Fabric unter Windows Server 2016](service-fabric-get-started-containers.md)
* [Bereitstellen eines Docker-Containers in Service Fabric unter Linux](service-fabric-get-started-containers-linux.md)

