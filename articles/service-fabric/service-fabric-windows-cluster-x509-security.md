---
title: Sichern eines Azure Service Fabric-Clusters unter Windows mithilfe von Zertifikaten | Microsoft Docs
description: "In diesem Artikel wird beschrieben, wie Sie die Kommunikation in einem eigenständigen oder privaten Cluster und zwischen Clients und dem Cluster schützen."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: fe0ed74c-9af5-44e9-8d62-faf1849af68c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dekapur
ms.translationtype: HT
ms.sourcegitcommit: a9cfd6052b58fe7a800f1b58113aec47a74095e3
ms.openlocfilehash: ebac24385560377bac27a8b8c425323c57392bd2
ms.contentlocale: de-de
ms.lasthandoff: 08/12/2017

---
# <a name="secure-a-standalone-cluster-on-windows-using-x509-certificates"></a>Schützen eines eigenständigen Windows-Clusters mithilfe von X.509-Zertifikaten
In diesem Artikel wird beschrieben, wie Sie die Kommunikation zwischen den unterschiedlichen Knoten Ihres eigenständigen Windows-Clusters schützen und Clients, die eine Verbindung mit diesem Cluster herstellen, mit X.509-Zertifikaten authentifizieren. So wird sichergestellt, dass nur autorisierte Benutzer auf den Cluster und die bereitgestellten Anwendungen zugreifen und Verwaltungsaufgaben durchführen können.  Die Zertifikatsicherheit sollte auf dem Cluster aktiviert werden, wenn der Cluster erstellt wird.  

Weitere Informationen zur Clustersicherheit wie Knoten-zu-Knoten-Sicherheit, Client-zu-Knoten-Sicherheit und rollenbasierte Zugriffssteuerung finden Sie unter [Szenarien für die Clustersicherheit](service-fabric-cluster-security.md).

## <a name="which-certificates-will-you-need"></a>Welche Zertifikate werden benötigt?
Laden Sie als Erstes das [Paket mit dem eigenständigen Cluster](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) auf einen der Knoten im Cluster herunter. Das heruntergeladene Paket enthält die Datei **ClusterConfig.X509.MultiMachine.json** . Öffnen Sie die Datei, und sehen Sie sich den Abschnitt **security** im Abschnitt **properties** an:

```JSON
"security": {
    "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
    "ClusterCredentialType": "X509",
    "ServerCredentialType": "X509",
    "CertificateInformation": {
        "ClusterCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },        
        "ClusterCertificateCommonNames": {
            "CommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]"
            }
            ],
            "X509StoreName": "My"
        },
        "ServerCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },
        "ServerCertificateCommonNames": {
            "CommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]"
            }
            ],
            "X509StoreName": "My"
        },
        "ClientCertificateThumbprints": [
            {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": false
            },
            {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }
        ],
        "ClientCertificateCommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }
        ],
        "ReverseProxyCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },
        "ReverseProxyCertificateCommonNames": {
            "CommonNames": [
                {
                "CertificateCommonName": "[CertificateCommonName]"
                }
            ],
            "X509StoreName": "My"
        }
    }
},
```

In diesem Abschnitt werden die Zertifikate beschrieben, die Sie zum Schützen des eigenständigen Windows-Clusters benötigen. Wenn Sie ein Zertifikat für Cluster angeben, legen Sie den Wert von **ClusterCredentialType** auf _**X509**_ fest. Wenn Sie das Serverzertifikat für externe Verbindungen angeben, legen Sie **ServerCredentialType** auf _**X509**_ fest. Auch wenn es nicht zwingend erforderlich ist, empfehlen wir beide Zertifikate für einen ordnungsgemäß gesicherten Cluster. Wenn Sie diese Werte auf *X509* gesetzt haben, müssen Sie auch die entsprechenden Zertifikate angeben, weil Service Fabric andernfalls eine Ausnahme ausgelöst. In einigen Szenarien möchten Sie möglicherweise nur _ClientCertificateThumbprints_ oder _ReverseProxyCertificate_ angeben. Sie dürfen in diesen Szenarien _ClusterCredentialType_ oder _ServerCredentialType_ nicht auf _X509_ festlegen.


> [!NOTE]
> Ein [Fingerabdruck](https://en.wikipedia.org/wiki/Public_key_fingerprint) ist die primäre Identität eines Zertifikats. Lesen Sie [Abrufen des Fingerabdrucks eines Zertifikats](https://msdn.microsoft.com/library/ms734695.aspx) , um den Fingerabdruck der von Ihnen erstellten Zertifikate zu ermitteln.
> 
> 

In der folgenden Tabelle sind die Zertifikate aufgeführt, die Sie für die Einrichtung des Clusters benötigen:

| **CertificateInformation-Einstellung** | **Beschreibung** |
| --- | --- |
| ClusterCertificate |Empfohlen für die Testumgebung. Dieses Zertifikat ist erforderlich, um die Kommunikation zwischen den Knoten in einem Cluster zu schützen. Sie können zwei verschiedene Zertifikate verwenden: ein primäres Zertifikat und ein sekundäres Zertifikat für Upgrades. Legen Sie den Fingerabdruck des primären Zertifikats im Abschnitt **Thumbprint** und den Fingerabdruck des sekundären Zertifikats in den Variablen unter **ThumbprintSecondary** fest. |
| ClusterCertificateCommonNames |Empfohlen für die Produktionsumgebung. Dieses Zertifikat ist erforderlich, um die Kommunikation zwischen den Knoten in einem Cluster zu schützen. Sie können einen oder zwei allgemeine Clusterzertifikatnamen verwenden. |
| ServerCertificate |Empfohlen für die Testumgebung. Dieses Zertifikat wird dem Client angezeigt, wenn versucht wird, eine Verbindung mit diesem Cluster herzustellen. Der Einfachheit halber können Sie für *ClusterCertificate* und *ServerCertificate* dasselbe Zertifikat auswählen. Sie können zwei verschiedene Serverzertifikate verwenden: ein primäres Zertifikat und ein sekundäres Zertifikat für Upgrades. Legen Sie den Fingerabdruck des primären Zertifikats im Abschnitt **Thumbprint** und den Fingerabdruck des sekundären Zertifikats in den Variablen unter **ThumbprintSecondary** fest. |
| ServerCertificateCommonNames |Empfohlen für die Produktionsumgebung. Dieses Zertifikat wird dem Client angezeigt, wenn versucht wird, eine Verbindung mit diesem Cluster herzustellen. Der Einfachheit halber können Sie für *ClusterCertificateCommonNames* und *ServerCertificateCommonNames* das gleiche Zertifikat auswählen. Sie können einen oder zwei allgemeine Serverzertifikatnamen verwenden. |
| ClientCertificateThumbprints |Dies ist eine Gruppe von Zertifikaten, die auf den authentifizierten Clients installiert werden sollen. Auf den Computern, denen Sie Zugriff auf den Cluster gewähren möchten, können Sie verschiedene Clientzertifikate installieren. Legen Sie den Fingerabdruck eines Zertifikats jeweils in der Variablen **CertificateThumbprint** fest. Wenn Sie **IsAdmin** auf *true*festlegen, kann der Client, auf dem dieses Zertifikat installiert ist, Administratorverwaltungsaktivitäten für den Cluster durchführen. Wenn **IsAdmin** auf *false*festgelegt ist, kann der Client mit diesem Zertifikat nur Aktionen durchführen, die für Benutzerzugriffsrechte zulässig sind. (Hierbei handelt es sich in der Regel um schreibgeschützten Zugriff.) Weitere Informationen zu Rollen finden Sie unter [Rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac) |
| ClientCertificateCommonNames |Legen Sie den allgemeinen Namen des ersten Clientzertifikats für **CertificateCommonName**fest. **CertificateIssuerThumbprint** ist der Fingerabdruck für den Aussteller dieses Zertifikats. Weitere Informationen zu allgemeinen Namen und zum Aussteller finden Sie unter [Verwenden von Zertifikaten](https://msdn.microsoft.com/library/ms731899.aspx) . |
| ReverseProxyCertificate |Empfohlen für die Testumgebung. Hierbei handelt es sich um ein optionales Zertifikat, das zum Schutz des [Reverseproxys](service-fabric-reverseproxy.md) angegeben werden kann. Stellen Sie bei Verwendung dieses Zertifikats sicher, dass „reverseProxyEndpointPort“ unter „nodeTypes“ festgelegt ist. |
| ReverseProxyCertificateCommonNames |Empfohlen für die Produktionsumgebung. Hierbei handelt es sich um ein optionales Zertifikat, das zum Schutz des [Reverseproxys](service-fabric-reverseproxy.md) angegeben werden kann. Stellen Sie bei Verwendung dieses Zertifikats sicher, dass „reverseProxyEndpointPort“ unter „nodeTypes“ festgelegt ist. |

Im Anschluss sehen Sie ein Beispiel für eine Clusterkonfiguration mit Cluster-, Server- und Clientzertifikaten: Beachten Sie, dass Fingerabdruck und allgemeiner Name bei Cluster-/Server-/Reverseproxyzertifikaten nicht gemeinsam für den gleichen Zertifikattyp konfiguriert werden dürfen.

 ```JSON
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace the localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "metadata": "Replace the localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "10.7.0.6",
        "metadata": "Replace the localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        }
        "security": {
            "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
            "ClusterCredentialType": "X509",
            "ServerCredentialType": "X509",
            "CertificateInformation": {
                "ClusterCertificateCommonNames": {
                  "CommonNames": [
                    {
                      "CertificateCommonName": "myClusterCertCommonName"
                    }
                  ],
                  "X509StoreName": "My"
                },
                "ServerCertificateCommonNames": {
                  "CommonNames": [
                    {
                      "CertificateCommonName": "myServerCertCommonName"
                    }
                  ],
                  "X509StoreName": "My"
                },
                "ClientCertificateThumbprints": [{
                    "CertificateThumbprint": "c4 c18 8e aa a8 58 77 98 65 f8 61 4a 0d da 4c 13 c5 a1 37 6e",
                    "IsAdmin": false
                }, {
                    "CertificateThumbprint": "71 de 04 46 7c 9e d0 54 4d 02 10 98 bc d4 4c 71 e1 83 41 4e",
                    "IsAdmin": true
                }]
            }
        },
        "reliabilityLevel": "Bronze",
        "nodeTypes": [{
            "name": "NodeType0",
            "clientConnectionEndpointPort": "19000",
            "clusterConnectionEndpointPort": "19001",
            "leaseDriverEndpointPort": "19002",
            "serviceConnectionEndpointPort": "19003",
            "httpGatewayEndpointPort": "19080",
            "applicationPorts": {
                "startPort": "20001",
                "endPort": "20031"
            },
            "ephemeralPorts": {
                "startPort": "20032",
                "endPort": "20062"
            },
            "isPrimary": true
        }
         ],
        "fabricSettings": [{
            "name": "Setup",
            "parameters": [{
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
            }, {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
            }]
        }]
    }
}
 ```

## <a name="certificate-roll-over"></a>Zertifikatrollover
Wenn Sie anstelle des Fingerabdrucks den allgemeinen Namen des Zertifikats verwenden, ist für das Zertifikatrollover kein Upgrade der Clusterkonfiguration erforderlich.
Wenn im Rahmen des Zertifikatrollovers auch ein Ausstellerrollover erfolgen soll, bewahren Sie das alte Ausstellerzertifikat nach der Installation des neuen Ausstellerzertifikats noch mindestens zwei Stunden im Zertifikatspeicher auf.

## <a name="acquire-the-x509-certificates"></a>Erwerben der X.509-Zertifikate
Zum Schutz der Kommunikation zwischen Clustern müssen Sie zuerst X.509-Zertifikate für die Clusterknoten abrufen. Um die Verbindungsherstellung für diesen Cluster auf autorisierte Computer und Benutzer zu beschränken, müssen Sie Zertifikate für die Clientcomputer abrufen und installieren.

Für Cluster, die Produktionsworkloads ausführen, sollte zum Schutz des Clusters ein von einer [Zertifizierungsstelle](https://en.wikipedia.org/wiki/Certificate_authority) signiertes X.509-Zertifikat verwendet werden. Ausführliche Informationen zum Abrufen dieser Zertifikate finden Sie unter [Vorgehensweise: Abrufen eines Zertifikats (WCF)](http://msdn.microsoft.com/library/aa702761.aspx).

Für Testcluster können Sie ein selbstsigniertes Zertifikat verwenden.

## <a name="optional-create-a-self-signed-certificate"></a>Optional: Erstellen eines selbstsignierten Zertifikats
Ein selbstsigniertes Zertifikat, das ordnungsgemäß geschützt werden kann, können Sie beispielsweise mithilfe des Skripts *CertSetup.ps1* im Service Fabric SDK-Ordner im Verzeichnis *C:\Programme\Microsoft SDKs\Service Fabric\ClusterSetup\Secure* erstellen. Bearbeiten Sie diese Datei, um den Standardnamen des Zertifikats zu ändern (suchen Sie nach dem Wert *CN = ServiceFabricDevClusterCert*). Führen Sie dieses Skript als `.\CertSetup.ps1 -Install` aus.

Exportieren Sie das Zertifikat nun in eine PFX-Datei mit einem geschützten Kennwort. Rufen Sie zuerst den Zertifikatfingerabdruck ab. Führen Sie im *Startmenü* den Befehl *Computerzertifikate verwalten* aus. Navigieren Sie im Ordner **Lokaler Computer\Persönlich** zum soeben erstellten Zertifikat. Doppelklicken Sie auf das Zertifikat, um es zu öffnen. Wählen Sie die Registerkarte *Details* aus, und scrollen Sie nach unten zum Feld *Fingerabdruck*. Kopieren Sie den Fingerabdruckwert ohne Leerzeichen in den folgenden PowerShell-Befehl.  Ändern Sie den `String`-Wert in ein geeignetes sicheres Kennwort, um ihn zu schützen, und führen Sie Folgendes in PowerShell aus:

```powershell   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

Die Details eines auf dem Computer installierten Zertifikats können Sie mithilfe des folgenden PowerShell-Befehls anzeigen:

```powershell
$cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
Write-Host $cert.ToString($true)
```

Wenn Sie über ein Azure-Abonnement verfügen, führen Sie alternativ dazu die Schritte im Abschnitt [Hinzufügen von Zertifikaten zum Schlüsseltresor](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault) aus.

## <a name="install-the-certificates"></a>Installieren der Zertifikate
Wenn Sie über Zertifikate verfügen, können Sie sie auf den Clusterknoten installieren. Auf den Knoten muss die neueste Windows PowerShell 3.x-Version installiert sein. Die Schritte müssen auf jedem Knoten sowohl für Clusterzertifikate als auch für Serverzertifikate und die jeweils dazugehörigen sekundären Zertifikate wiederholt werden.

1. Kopieren Sie die PFX-Datei(en) auf den Knoten.
2. Öffnen Sie ein PowerShell-Fenster als Administrator, und geben Sie die folgenden Befehle ein. Ersetzen Sie *$pswd* durch das Kennwort, das Sie bei der Zertifikaterstellung verwendet haben. Ersetzen Sie *$PfxFilePath* durch den vollständigen Pfad der auf diesen Knoten kopierten PFX-Datei.
   
    ```powershell
    $pswd = "1234"
    $PfxFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```
3. Als Nächstes muss die Zugriffssteuerung für dieses Zertifikat durch Ausführen des folgenden Skripts so festgelegt werden, dass der unter dem Netzwerkdienstkonto ausgeführte Service Fabric-Prozess sie nutzen kann. Geben Sie den Fingerabdruck des Zertifikats und „NETWORK SERVICE“ für das Dienstkonto an. Sie können überprüfen, ob die ACLs für das Zertifikat richtig sind, indem Sie das Zertifikat in *Start* > *Computerzertifikate verwalten* öffnen und *Alle Aufgaben* > *Private Schlüssel verwalten* ansehen.
   
    ```powershell
    param
    (
    [Parameter(Position=1, Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [string]$pfxThumbPrint,
   
    [Parameter(Position=2, Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [string]$serviceAccount
    )
   
    $cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object -FilterScript { $PSItem.ThumbPrint -eq $pfxThumbPrint; }
   
    # Specify the user, the permissions and the permission type
    $permission = "$($serviceAccount)","FullControl","Allow"
    $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission
   
    # Location of the machine related keys
    $keyPath = Join-Path -Path $env:ProgramData -ChildPath "\Microsoft\Crypto\RSA\MachineKeys"
    $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
    $keyFullPath = Join-Path -Path $keyPath -ChildPath $keyName
   
    # Get the current acl of the private key
    $acl = (Get-Item $keyFullPath).GetAccessControl('Access')
   
    # Add the new ace to the acl of the private key
    $acl.SetAccessRule($accessRule)
   
    # Write back the new acl
    Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop
   
    # Observe the access rights currently assigned to this certificate.
    get-acl $keyFullPath| fl
    ```
4. Wiederholen Sie die obigen Schritte für jedes Serverzertifikat. Mit diesen Schritten können Sie auch die Clientzertifikate auf den Computern installieren, denen Sie Zugriff auf den Cluster gewähren möchten.

## <a name="create-the-secure-cluster"></a>Erstellen des geschützten Clusters
Nach dem Konfigurieren des Abschnitts **security** der Datei **ClusterConfig.X509.MultiMachine.json** können Sie mit dem Abschnitt [Erstellen Ihres Clusters](service-fabric-cluster-creation-for-windows-server.md#createcluster) fortfahren, um die Knoten zu konfigurieren und den eigenständigen Cluster zu erstellen. Denken Sie beim Erstellen des Clusters daran, die Datei **ClusterConfig.X509.MultiMachine.json** zu verwenden. Der Befehl kann beispielsweise wie folgt aussehen:

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

Wenn der gesicherte eigenständige Windows-Cluster ausgeführt wird und Sie die authentifizierten Clients für die Verbindungsherstellung eingerichtet haben, geht es mit dem Abschnitt [Herstellen einer Verbindung mit einem sicheren Cluster mit PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) weiter, um die Verbindung herzustellen. Beispiel:

```powershell
$ConnectArgs = @{  ConnectionEndpoint = '10.7.0.5:19000';  X509Credential = $True;  StoreLocation = 'LocalMachine';  StoreName = "MY";  ServerCertThumbprint = "057b9544a6f2733e0c8d3a60013a58948213f551";  FindType = 'FindByThumbprint';  FindValue = "057b9544a6f2733e0c8d3a60013a58948213f551"   }
Connect-ServiceFabricCluster $ConnectArgs
```

Sie können dann andere PowerShell-Befehle zum Arbeiten mit diesem Cluster ausführen. Verwenden Sie beispielsweise [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode.md?view=azureservicefabricps), um eine Liste von Knoten in diesem sicheren Cluster anzuzeigen.


Um den Cluster zu entfernen, stellen Sie eine Verbindung mit dem Knoten im Cluster her, in dem Sie das Service Fabric-Paket heruntergeladen haben, öffnen Sie eine Befehlszeile, und navigieren Sie zum Paketordner. Führen Sie jetzt den folgenden Befehl aus:

```powershell
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

> [!NOTE]
> Eine fehlerhafte Zertifikatkonfiguration kann dazu führen, dass der Cluster während der Bereitstellung nicht angezeigt wird. Um Sicherheitsprobleme selbst zu diagnostizieren, sehen Sie in der Ereignisanzeigegruppe *Anwendungs- und Dienstprotokolle* > *Microsoft-Service Fabric* nach.
> 
> 


