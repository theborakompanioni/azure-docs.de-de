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
   ms.date="06/10/2016"
   ms.author="dkshir"/>

# Schützen des eigenständigen Windows-Clusters mit Zertifikaten

In diesem Artikel wird beschrieben, wie Sie die Kommunikation zwischen den unterschiedlichen Knoten Ihres eigenständigen Windows-Clusters schützen und Clients, die eine Verbindung mit diesem Cluster herstellen, mit X.509-Zertifikaten authentifizieren. So wird sichergestellt, dass nur autorisierte Benutzer auf den Cluster zugreifen, Anwendungen bereitstellen und Verwaltungsaufgaben ausführen können. Die Zertifikatsicherheit sollte auf dem Cluster aktiviert werden, wenn der Cluster erstellt wird. Weitere Informationen zur Knoten-zu-Knoten-Sicherheit, Client-zu-Knoten-Sicherheit und rollenbasierten Zugriffssteuerung finden Sie unter [Szenarien für die Clustersicherheit](service-fabric-cluster-security.md).

## Welche Zertifikate werden benötigt?

[Laden Sie als Erstes das Paket mit dem eigenständigen Cluster auf einen der Knoten im Cluster herunter](service-fabric-cluster-creation-for-windows-server.md#downloadpackage). Das heruntergeladene Paket enthält die Datei **ClusterConfig.X509.json**. Öffnen Sie die Datei im Modus *Bearbeiten*, und sehen Sie sich auf der Registerkarte **properties** den Abschnitt **security** an:

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

In diesem Abschnitt werden alle Zertifikate beschrieben, die Sie zum Schützen des eigenständigen Windows-Clusters benötigen. Aktivieren Sie die zertifikatbasierte Sicherheit, indem Sie die Werte von **ClusterCredentialType** und **ServerCredentialType** auf *X509* festlegen.

>[AZURE.NOTE] Ein [Fingerabdruck](https://en.wikipedia.org/wiki/Public_key_fingerprint) ist die primäre Identität eines Zertifikats. Lesen Sie sich [Vorgehensweise: Abrufen des Fingerabdrucks eines Zertifikats](https://msdn.microsoft.com/library/ms734695(v=vs.110).aspx) durch, um den Fingerabdruck der von Ihnen erstellten Zertifikate zu ermitteln.

In der folgenden Tabelle sind die tatsächlichen Zertifikate aufgeführt, die Sie für die Einrichtung des Clusters benötigen:

|**CertificateInformation-Einstellung**|**Beschreibung**|
|-----------------------|--------------------------|
|ClusterCertificate|Dieses Zertifikat ist erforderlich, um die Kommunikation zwischen den Knoten in einem Cluster zu schützen. Sie können zwei verschiedene Zertifikate verwenden: ein primäres Zertifikat und ein sekundäres Zertifikat für das Failover. Legen Sie den Fingerabdruck des primären Zertifikats im Abschnitt **Thumbprint** und den Fingerabdruck des sekundären Zertifikats in den Variablen unter **ThumbprintSecondary** fest.|
|ServerCertificate|Dieses Zertifikat wird dem Client angezeigt, wenn versucht wird, eine Verbindung mit diesem Cluster herzustellen. Der Einfachheit halber können Sie für *ClusterCertificate* und *ServerCertificate* dasselbe Zertifikat auswählen. Sie können zwei verschiedene Serverzertifikate verwenden: ein primäres Zertifikat und ein sekundäres Zertifikat für das Failover. Legen Sie den Fingerabdruck des primären Zertifikats im Abschnitt **Thumbprint** und den Fingerabdruck des sekundären Zertifikats in den Variablen unter **ThumbprintSecondary** fest. |
|ClientCertificateThumbprints|Dies ist eine Gruppe von Zertifikaten, die auf den authentifizierten Clients installiert werden sollen. Auf den Computern, denen Sie Zugriff auf den Cluster und die darauf ausgeführten Anwendungen gewähren möchten, können Sie verschiedene Clientzertifikate installieren. Legen Sie den Fingerabdruck eines Zertifikats jeweils in der Variablen **CertificateThumbprint** fest. Wenn Sie **IsAdmin** auf *true* festlegen, kann der Client, auf dem dieses Zertifikat installiert ist, verschiedene Verwaltungsaktivitäten für den Cluster durchführen. Wenn **IsAdmin** auf *false* festgelegt ist, besteht nur Zugriff auf die Anwendungen, die im Cluster ausgeführt werden.|
|ClientCertificateCommonNames|Legen Sie den allgemeinen Namen des ersten Clientzertifikats für **CertificateCommonName** fest. **CertificateIssuerThumbprint** ist der Fingerabdruck für den Aussteller dieses Zertifikats. Lesen Sie sich [Verwenden von Zertifikaten](https://msdn.microsoft.com/library/ms731899(v=vs.110).aspx) durch, um weitere Informationen zu allgemeinen Namen und zum Aussteller zu erhalten.|


## Installieren der Zertifikate

Zum Schützen der Kommunikation zwischen Clustern müssen Sie zuerst X.509-Zertifikate für die Clusterknoten beschaffen. Um die Verbindungsherstellung für diesen Cluster auf autorisierte Computer und Benutzer zu beschränken, müssen Sie Zertifikate für diese potenziellen Clientcomputer abrufen und installieren. Die Schritte zum Abrufen von X.509-Zertifikaten finden Sie unter [Vorgehensweise: Abrufen eines Zertifikats](https://msdn.microsoft.com/library/aa702761.aspx). Sie müssen ein **PFX**-Zertifikat erstellen, damit Sie Ihren privaten Schlüssel speichern können. Wenn Sie über ein Azure-Abonnement verfügen, können Sie alternativ dazu die Schritte unter [Beschaffen von X.509-Zertifikaten](service-fabric-secure-azure-cluster-with-certs.md#acquirecerts) ausführen, um Zertifikate zu erstellen. Nachdem Sie dieses Zertifikat erhalten haben, können Sie es mit den folgenden Schritten auf den Clusterknoten installieren. Beachten Sie, dass bei den Schritten vorausgesetzt wird, dass auf den Knoten bereits die aktuelle Windows PowerShell 3.x-Version installiert ist. Sie müssen diese Schritte auf jedem Knoten sowohl für Clusterzertifikate als auch für Serverzertifikate und die jeweils dazugehörigen sekundären Zertifikate wiederholen.

- Kopieren Sie die PFX-Datei auf den Knoten.

- Öffnen Sie ein PowerShell-Fenster als Administrator, und geben Sie die folgenden Befehle ein:
	
		Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My `
		-FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $password -AsPlainText -Force)

Ersetzen Sie *$password* durch das Kennwort, das Sie zum Erstellen des Zertifikats verwendet haben. Ersetzen Sie $PfxFilePath durch den vollständigen Pfad der auf diesen Knoten kopierten PFX-Datei.

- Legen Sie die Zugriffssteuerung für dieses Zertifikat so fest, dass der Service Fabric-Prozess sie nutzen kann, indem Sie das folgende Skript ausführen:

		$cert = get-item Cert:\LocalMachine\My<CertificateThumbprint>
		$cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName

		#Note down the string output by this command, this is the container name for your private key. 

		$privateKeyFilePath = 'C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys<PrivateKeyContainerName>'
	
		#Observe the access rights currently assigned to this certificate.
		get-acl $privateKeyFilePath | fl

		#Set the ACL so that the Service Fabric process can access it.
		$acl = get-acl $privateKeyFilePath
		$acl.SetAccessRule((New-Object System.Security.Accesscontrol.FileSystemAccessRule("NT AUTHORITY\NetworkService","FullControl","Allow")))
		Set-Acl $privateKeyFilePath $acl -ErrorAction Stop 
	

Nutzen Sie diese Schritte auch, um die Clientzertifikate auf den Computern zu installieren, denen Sie Zugriff auf den Cluster gewähren möchten.


## Nächste Schritte

Nach dem Konfigurieren des Abschnitts **security** der Datei „ClusterConfig.X509.json“ können Sie mit dem Abschnitt zum [Erstellen Ihres Clusters](service-fabric-cluster-creation-for-windows-server.md#createcluster) fortfahren, um die Knoten zu konfigurieren und den eigenständigen Cluster zu erstellen. Denken Sie beim Erstellen des Clusters an die Verwendung der Datei **ClusterConfig.X509.json**. Der Befehl kann beispielsweise wie folgt aussehen:

	cd $ServiceFabricDeployAnywhereFolder
	.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.json -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab -AcceptEULA $true -Verbose


Nachdem der geschützte eigenständige Windows-Cluster erfolgreich ausgeführt wird und Sie die authentifizierten Clients für die Verbindungsherstellung eingerichtet haben, geht es mit dem Abschnitt [Herstellen einer Verbindung mit einem sicheren Cluster mit PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) weiter, um die Verbindung herzustellen.

<!---HONumber=AcomDC_0615_2016-->