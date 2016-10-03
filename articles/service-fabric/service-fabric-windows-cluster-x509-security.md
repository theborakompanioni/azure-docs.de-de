<properties
   pageTitle="Herstellen einer Verbindung mit einem geschützten privaten Cluster | Microsoft Azure"
   description="In diesem Artikel wird beschrieben, wie Sie die Kommunikation in einem eigenständigen oder privaten Cluster und zwischen Clients und dem Cluster schützen."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/08/2016"
   ms.author="dkshir"/>

# Schützen eines eigenständigen Windows-Clusters mithilfe von X.509-Zertifikaten

In diesem Artikel wird beschrieben, wie Sie die Kommunikation zwischen den unterschiedlichen Knoten Ihres eigenständigen Windows-Clusters schützen und Clients, die eine Verbindung mit diesem Cluster herstellen, mit X.509-Zertifikaten authentifizieren. So wird sichergestellt, dass nur autorisierte Benutzer auf den Cluster und die bereitgestellten Anwendungen zugreifen und Verwaltungsaufgaben durchführen können. Die Zertifikatsicherheit sollte auf dem Cluster aktiviert werden, wenn der Cluster erstellt wird.

Weitere Informationen zur Clustersicherheit wie Knoten-zu-Knoten-Sicherheit, Client-zu-Knoten-Sicherheit und rollenbasierte Zugriffssteuerung finden Sie unter [Szenarien für die Clustersicherheit](service-fabric-cluster-security.md).

## Welche Zertifikate werden benötigt?

Laden Sie als Erstes das [Paket mit dem eigenständigen Cluster](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) auf einen der Knoten im Cluster herunter. Das heruntergeladene Paket enthält die Datei **ClusterConfig.X509.MultiMachine.json**. Öffnen Sie die Datei, und sehen Sie im Abschnitt **properties** den Abschnitt **security** an:

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
            "ServerCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ClientCertificateThumbprints": [{
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": false
            }, {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }],
            "ClientCertificateCommonNames": [{
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint" : "[Thumbprint]",
                "IsAdmin": true
            }]
        }
    },

In diesem Abschnitt werden die Zertifikate beschrieben, die Sie zum Schützen des eigenständigen Windows-Clusters benötigen. Aktivieren Sie die zertifikatbasierte Sicherheit, indem Sie die Werte von **ClusterCredentialType** und **ServerCredentialType** auf *X509* festlegen.

>[AZURE.NOTE] Ein [Fingerabdruck](https://en.wikipedia.org/wiki/Public_key_fingerprint) ist die primäre Identität eines Zertifikats. Lesen Sie [Abrufen des Fingerabdrucks eines Zertifikats](https://msdn.microsoft.com/library/ms734695.aspx), um den Fingerabdruck der von Ihnen erstellten Zertifikate zu ermitteln.

In der folgenden Tabelle sind die Zertifikate aufgeführt, die Sie für die Einrichtung des Clusters benötigen:

|**CertificateInformation-Einstellung**|**Beschreibung**|
|-----------------------|--------------------------|
|ClusterCertificate|Dieses Zertifikat ist erforderlich, um die Kommunikation zwischen den Knoten in einem Cluster zu schützen. Sie können zwei verschiedene Zertifikate verwenden: ein primäres Zertifikat und ein sekundäres Zertifikat für Upgrades. Legen Sie den Fingerabdruck des primären Zertifikats im Abschnitt **Thumbprint** und den Fingerabdruck des sekundären Zertifikats in den Variablen unter **ThumbprintSecondary** fest.|
|ServerCertificate|Dieses Zertifikat wird dem Client angezeigt, wenn versucht wird, eine Verbindung mit diesem Cluster herzustellen. Der Einfachheit halber können Sie für *ClusterCertificate* und *ServerCertificate* dasselbe Zertifikat auswählen. Sie können zwei verschiedene Serverzertifikate verwenden: ein primäres Zertifikat und ein sekundäres Zertifikat für Upgrades. Legen Sie den Fingerabdruck des primären Zertifikats im Abschnitt **Thumbprint** und den Fingerabdruck des sekundären Zertifikats in den Variablen unter **ThumbprintSecondary** fest. |
|ClientCertificateThumbprints|Dies ist eine Gruppe von Zertifikaten, die auf den authentifizierten Clients installiert werden sollen. Auf den Computern, denen Sie Zugriff auf den Cluster gewähren möchten, können Sie verschiedene Clientzertifikate installieren. Legen Sie den Fingerabdruck eines Zertifikats jeweils in der Variablen **CertificateThumbprint** fest. Wenn Sie **IsAdmin** auf *true* festlegen, kann der Client, auf dem dieses Zertifikat installiert ist, Administratorverwaltungsaktivitäten für den Cluster durchführen. Wenn **IsAdmin** auf *false* festgelegt ist, kann der Client mit diesem Zertifikat nur Aktionen durchführen, die für Benutzerzugriffsrechte zulässig sind. (Hierbei handelt es sich in der Regel um schreibgeschützten Zugriff.) Weitere Informationen zu Rollen finden Sie unter [Rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC)](service-fabric-cluster-security.md/#role-based-access-control-rbac). |
|ClientCertificateCommonNames|Legen Sie den allgemeinen Namen des ersten Clientzertifikats für **CertificateCommonName** fest. **CertificateIssuerThumbprint** ist der Fingerabdruck für den Aussteller dieses Zertifikats. Weitere Informationen zu allgemeinen Namen und zum Aussteller finden Sie unter [Verwenden von Zertifikaten](https://msdn.microsoft.com/library/ms731899.aspx).|

Im Anschluss sehen Sie ein Beispiel für eine Clusterkonfiguration mit Cluster-, Server- und Clientzertifikaten:

 ```
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
                "ClusterCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ServerCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
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
            "clusterConnectionEndpoint": "19001",
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

## Beschaffen der X.509-Zertifikate
Zum Schutz der Kommunikation zwischen Clustern müssen Sie zuerst X.509-Zertifikate für die Clusterknoten abrufen. Um die Verbindungsherstellung für diesen Cluster auf autorisierte Computer und Benutzer zu beschränken, müssen Sie Zertifikate für die Clientcomputer abrufen und installieren.

Für Cluster, die Produktionsworkloads ausführen, sollte zum Schutz des Clusters ein von einer [Zertifizierungsstelle](https://en.wikipedia.org/wiki/Certificate_authority) signiertes X.509-Zertifikat verwendet werden. Ausführliche Informationen zum Abrufen dieser Zertifikate finden Sie unter [Vorgehensweise: Abrufen eines Zertifikats (WCF)](http://msdn.microsoft.com/library/aa702761.aspx).

Für Testcluster können Sie ein selbstsigniertes Zertifikat verwenden.

## Optional: Erstellen eines selbstsignierten Zertifikats
Ein selbstsigniertes Zertifikat, das korrekt geschützt werden kann, können Sie beispielsweise mithilfe des Skripts *CertSetup.ps1* aus dem Service Fabric SDK-Ordner im Verzeichnis *C:\\Programme\\Microsoft SDKs\\Service Fabric\\ClusterSetup\\Secure* erstellen. Bearbeiten Sie diese Datei, und verwenden Sie das Ergebnis, um ein Zertifikat mit einem geeigneten Namen zu erstellen.

Exportieren Sie das Zertifikat nun in eine PFX-Datei mit einem geschützten Kennwort. Zuerst müssen Sie den Fingerabdruck des Zertifikats abrufen. Führen Sie die Anwendung „certmgr.exe“ aus. Navigieren Sie im Ordner **Lokaler Computer\\Persönlich** zum soeben erstellten Zertifikat. Doppelklicken Sie auf das Zertifikat, um es zu öffnen. Wählen Sie die Registerkarte *Details* aus, und scrollen Sie nach unten zum Feld *Fingerabdruck*. Kopieren Sie den Fingerabdruckwert ohne Leerzeichen in den folgenden PowerShell-Befehl. Ändern Sie den Wert *$pswd* zum Schutz in ein geeignetes sicheres Kennwort, und führen Sie den PowerShell-Befehl aus:

```   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

Die Details eines auf dem Computer installierten Zertifikats können Sie mithilfe des folgenden PowerShell-Befehls anzeigen:

```
$cert = Get-Item Cert:\LocalMachine\My<Thumbprint>
Write-Host $cert.ToString($true)
```

Wenn Sie über ein Azure-Abonnement verfügen, können Sie alternativ die Schritte unter [Beschaffen der X.509-Zertifikate](service-fabric-secure-azure-cluster-with-certs.md#acquirecerts) ausführen, um Ihre Zertifikate zu erstellen.

## Installieren der Zertifikate
Wenn Sie über Zertifikate verfügen, können Sie sie auf den Clusterknoten installieren. Auf den Knoten muss die neueste Windows PowerShell 3.x-Version installiert sein. Die Schritte müssen auf jedem Knoten sowohl für Clusterzertifikate als auch für Serverzertifikate und die jeweils dazugehörigen sekundären Zertifikate wiederholt werden.

1. Kopieren Sie die PFX-Datei(en) auf den Knoten.
2. Öffnen Sie ein PowerShell-Fenster als Administrator, und geben Sie die folgenden Befehle ein. Ersetzen Sie *$pswd* durch das Kennwort, das Sie bei der Zertifikaterstellung verwendet haben. Ersetzen Sie *$PfxFilePath* durch den vollständigen Pfad der auf diesen Knoten kopierten PFX-Datei.

    ```
    $pswd = "1234"
    $PfcFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```

3. Als Nächstes muss die Zugriffssteuerung für dieses Zertifikat durch Ausführen des folgenden Skripts so festgelegt werden, dass der unter dem Netzwerkdienstkonto ausgeführte Service Fabric-Prozess sie nutzen kann: Geben Sie den Fingerabdruck des Zertifikats und „NETWORK SERVICE“ für das Dienstkonto an. Mithilfe des Tools „certmgr.exe“ und durch Anzeigen von „Private Schlüssel verwalten“ können Sie sich vergewissern, dass die ACLs für das Zertifikat korrekt sind.

    ```
    param
    (
        [Parameter(Position=1, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$pfxThumbPrint,

        [Parameter(Position=2, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$serviceAccount
        )

        $cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object -FilterScript { $PSItem.ThumbPrint -eq $pfxThumbPrint; };

        # Specify the user, the permissions and the permission type
        $permission = "$($serviceAccount)","FullControl","Allow"
        $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission;

        # Location of the machine related keys
        $keyPath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys";
        $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName;
        $keyFullPath = $keyPath + $keyName;

        # Get the current acl of the private key
        $acl = (Get-Item $keyFullPath).GetAccessControl('Access')

        # Add the new ace to the acl of the private key
        $acl.SetAccessRule($accessRule);

        # Write back the new acl
        Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop

        #Observe the access rights currently assigned to this certificate.
        get-acl $keyFullPath| fl
        ```

4. Wiederholen Sie die obigen Schritte für jedes Serverzertifikat. Mit diesen Schritten können Sie auch die Clientzertifikate auf den Computern installieren, denen Sie Zugriff auf den Cluster gewähren möchten.

## Erstellen des geschützten Clusters
Nach dem Konfigurieren des Abschnitts **security** der Datei **ClusterConfig.X509.MultiMachine.json** können Sie mit dem Abschnitt [Erstellen Ihres Clusters](service-fabric-cluster-creation-for-windows-server.md#createcluster) fortfahren, um die Knoten zu konfigurieren und den eigenständigen Cluster zu erstellen. Denken Sie beim Erstellen des Clusters an die Verwendung der Datei **ClusterConfig.X509.MultiMachine.json**. Der Befehl kann beispielsweise wie folgt aussehen:

```
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab -AcceptEULA $true
```

Wenn der geschützte eigenständige Windows-Cluster ausgeführt wird und Sie die authentifizierten Clients für die Verbindungsherstellung eingerichtet haben, geht es mit dem Abschnitt [Herstellen einer Verbindung mit einem sicheren Cluster mit PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) weiter, um die Verbindung herzustellen. Beispiel:

```
Connect-ServiceFabricCluster -ConnectionEndpoint 10.7.0.4:19000 -KeepAliveIntervalInSec 10 -X509Credential -ServerCertThumbprint 057b9544a6f2733e0c8d3a60013a58948213f551 -FindType FindByThumbprint -FindValue 057b9544a6f2733e0c8d3a60013a58948213f551 -StoreLocation CurrentUser -StoreName My
```

Wenn Sie bei einem der Computer im Cluster angemeldet sind, können Sie einfach den PowerShell-Befehl ausführen, um die Verbindung mit dem Cluster herzustellen und eine Liste mit Knoten anzuzeigen, da das Zertifikat bereits lokal installiert ist:

```
Connect-ServiceFabricCluster
Get-ServiceFabricNode
```
Rufen Sie zum Entfernen des Clusters den folgenden Befehl auf:

```
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json   -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab
```

<!---HONumber=AcomDC_0921_2016-->