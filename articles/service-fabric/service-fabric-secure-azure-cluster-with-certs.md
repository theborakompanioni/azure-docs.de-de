<properties
   pageTitle="Schützen eines Service Fabric-Clusters mit Zertifikaten | Microsoft Azure"
   description="Sichern eines Service Fabric-Clusters mit X.509-Zertifikaten."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/27/2016"
   ms.author="chackdan"/>

# Schützen eines Service Fabric-Clusters in Azure mit Zertifikaten

Ein Azure Service Fabric-Cluster ist eine Ressource, die sich in Ihrem Besitz befindet. Um den unbefugten Zugriff auf die Ressource zu verhindern, müssen Sie sie sichern. Dies gilt besonders, wenn darin Workloads für die Produktion ausgeführt werden. Zum Einrichten eines sicheren Service Fabric-Clusters mit X.509-Zertifikaten benötigen Sie mindestens ein X.509-Serverzertifikat, das Sie in Azure Key Vault hochladen und bei der Erstellung des Clusters verwenden.

Dieser Artikel entspricht [Schritt 3: Konfigurieren der Sicherheit](service-fabric-cluster-creation-via-portal.md#step-3--configure-security) im Clustererstellungsvorgang. Weitere Informationen zur Verwendung von X.509-Zertifikaten durch Service Fabric finden Sie unter [Szenarien für die Clustersicherheit](service-fabric-cluster-security.md).

Der Vorgang umfasst drei Schritte:

1. Erwerben des X.509-Zertifikats
2. Hochladen des Zertifikats in Azure Key Vault
3. Eingeben des Speicherorts und der Details zum Zertifikat beim Vorgang zur Erstellung des Service Fabric-Clusters.

## Schritt 1: Erwerben des X.509-Zertifikats

Für Cluster, die Produktionsworkloads ausführen, müssen Sie ein von der [Zertifizierungsstelle](https://en.wikipedia.org/wiki/Certificate_authority) signiertes X.509-Zertifikat zum Schutz des Clusters verwenden. Informationen zum Abrufen dieser Zertifikate finden Sie unter [Vorgehensweise: Abrufen eines Zertifikats](http://msdn.microsoft.com/library/aa702761.aspx).

Für Cluster, die Sie nur für Testzwecke verwenden, können Sie ein selbstsigniertes Zertifikat verwenden. In Schritt 2.5 unten wird dieser Vorgang erläutert.

## Schritt 2: Hochladen des X.509-Zertifikats in Key Vault

Dies ist ein komplexer Vorgang. Wir haben daher ein PowerShell-Modul in ein Git-Repository hochgeladen, mit dem dieser Vorgang für Sie durchgeführt wird.

### Schritt 2.1
Stellen Sie sicher, dass mindestens Azure PowerShell 1.0 auf Ihrem Computer installiert ist. Ist dies nicht der Fall, führen Sie die Schritte unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) aus.

### Schritt 2.2
Kopieren Sie den Ordner *ServiceFabricRPHelpers* aus diesem [Git-Repository](https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers) auf Ihren Computer.

### Schritt 2.3
Öffnen Sie ein PowerShell-Fenster, und wechseln Sie zu dem Verzeichnis, in das Sie das Modul heruntergeladen haben. Importieren Sie das Modul mit dem folgenden Befehl:

```powershell
Import-Module .\ServiceFabricRPHelpers.psm1
```

### Schritt 2.4
Wenn Sie ein Zertifikat verwenden, das Sie bereits bezogen haben, befolgen Sie das Verfahren in diesem Schritt. Fahren Sie andernfalls mit Schritt 2.5 fort, in dem erläutert wird, wie Sie das selbstsignierte Zertifikat im Schlüsseltresor erstellen und bereitstellen.

Sie können zum Speichern des Zertifikats eine vorhandene Ressourcengruppe und einen vorhandenen Schlüsseltresor verwenden. Oder erstellen Sie eine neue Ressourcengruppe und/oder einen Schlüsseltresor, sofern diese Elemente noch nicht vorhanden sind. Ein vorhandener Schlüsseltresor muss zunächst mithilfe dieses Skripts bereitstellungsfähig gemacht werden.

```powershell
Login-AzureRmAccount

Set-AzureRmKeyVaultAccessPolicy -VaultName <Name of the Vault> -ResourceGroupName <string> -EnabledForDeployment
```

Führen Sie zum Hochladen des Zertifikats in die Ressourcengruppe und den Schlüsseltresor das folgende Skript aus. Die Ressourcengruppe und der Schlüsseltresor werden erstellt, sofern sie noch nicht vorhanden sind.

```powershell
Login-AzureRmAccount
Invoke-AddCertToKeyVault -SubscriptionId <your subscription id> -ResourceGroupName <string> -Location <region> -VaultName <Name of the Vault> -CertificateName <Name of the Certificate> -Password <Certificate password> -UseExistingCertificate -ExistingPfxFilePath <Full path to the .pfx file>
```
Hier ist ein ausgefülltes Skript als Beispiel angegeben.

```powershell
Login-AzureRmAccount
Invoke-AddCertToKeyVault -SubscriptionId 35389201-c0b3-405e-8a23-9f1450994307 -ResourceGroupName chackdankeyvault4doc -Location westus -VaultName chackdankeyvault4doc  -CertificateName chackdantestcertificate2 -Password abcd123 -UseExistingCertificate -ExistingPfxFilePath C:\MyCertificates\ChackdanTestCertificate.pfx
```

Nach erfolgreichem Abschluss des Skripts erhalten Sie eine Ausgabe wie unten angegeben, die Sie für Schritt 3 (Einrichten eines sicheren Clusters) benötigen.

```
Certificate Thumbprint: 2118C3BCE6541A54A0236E14ED2CCDD77EA4567A

SourceVault /Resource ID of the key vault :  /subscriptions/35389201-c0b3-405e-8a23-9f1450994307/resourceGroups/chackdankeyvault4doc/providers/Microsoft.KeyVault/vaults/chackdankeyvault4doc

Certificate URL /URL to the certificate location in the key vault : https://chackdankeyvalut4doc.vault.azure.net:443/secrets/chackdantestcertificate3/ebc8df6300834326a95d05d90e0701ea
```

Jetzt verfügen Sie über die Informationen, die Sie zum Einrichten eines sicheren Clusters benötigen. Fahren Sie mit Schritt 3 fort.

### Schritt 2.5
Wenn Sie *kein* Zertifikat besitzen und ein neues selbstsigniertes Zertifikat erstellen und in den Schlüsseltresor hochladen möchten, gehen Sie wie folgt vor. Selbstsignierte Zertifikate sollten nur für Testcluster und nicht für Produktionscluster verwendet werden.

Sie können zum Speichern des Zertifikats eine vorhandene Ressourcengruppe und einen vorhandenen Schlüsseltresor verwenden. Oder erstellen Sie eine neue Ressourcengruppe und/oder einen Schlüsseltresor, sofern diese Elemente noch nicht vorhanden sind. Ein vorhandener Schlüsseltresor muss zunächst mithilfe dieses Skripts bereitstellungsfähig gemacht werden.

```powershell
Login-AzureRmAccount
Set-AzureRmKeyVaultAccessPolicy -VaultName <Name of the Vault> -ResourceGroupName <string> -EnabledForDeployment
```

Das folgende Skript erstellt eine neue Ressourcengruppe und/oder einen neuen Schlüsseltresor, sofern noch nicht vorhanden, erstellt ein selbstsigniertes Zertifikat und lädt es in den Schlüsseltresor hoch und gibt das neue Zertifikat an *OutputPath* aus.

```powershell
Login-AzureRmAccount
Invoke-AddCertToKeyVault -SubscriptionId <you subscription id> -ResourceGroupName <string> -Location <region> -VaultName <Name of the Vault> -CertificateName <Name of the Certificate> -Password <Certificate password> -CreateSelfSignedCertificate -DnsName <string- see note below.> -OutputPath <Full path to the .pfx file>
```
Die *DnsName*-Zeichenfolge gibt einen oder mehrere DNS-Namen an, die in die Erweiterung des alternativen Antragstellernamens (subject-alternative-name) des Zertifikats eingefügt werden, wenn ein zu kopierendes Zertifikat nicht über den CloneCert-Parameter angegeben wird. Der erste DNS-Name wird auch als Antragstellername gespeichert. Wenn kein Signaturzertifikat angegeben ist, wird der erste DNS-Name auch als Ausstellername gespeichert. Das *Invoke-AddCertToKeyVault*-Cmdlet verwendet für die Erstellung des selbstsignierten Zertifikats das Cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/library/hh848633.aspx).

Hier ist ein ausgefülltes Skript als Beispiel angegeben.

```powershell
Login-AzureRmAccount
Invoke-AddCertToKeyVault -SubscriptionId 35389201-c0b3-405e-8a23-9f1450994307 -ResourceGroupName chackdankeyvault4doc -Location westus -VaultName chackdankeyvault4doc  -CertificateName chackdantestcertificate3 -Password abcd123 -CreateSelfSignedCertificate -DnsName www.chackdan.westus.azure.com -OutputPath C:\MyCertificates
```

Nach dem erfolgreichen Abschluss des Skripts erhalten Sie eine Ausgabe wie unten angegeben. Diese benötigen Sie für Schritt 3.

```
Certificate Thumbprint: 64881409F4D86498C88EEC3697310C15F8F1540F

SourceVault /Resource ID of the key vault : /subscriptions/35389201-c0b3-405e-8a23-9f1450994307/resourceGroups/chackdankeyvault4doc/providers/Microsoft.KeyVault/vaults/chackdankeyvault4doc

Certificate URL /URL to the certificate location in the key vault: https://chackdankeyvalut4doc.vault.azure.net:443/secrets/chackdantestcertificate3/fvc8df6300834326a95d05d90e0720ea
```

## Schritt 3: Einrichten eines sicheren Clusters

Sobald Ihre Zertifikate in einen Azure-Schlüsseltresor hochgeladen wurden, können Sie einen mit diesen Zertifikaten gesicherten Cluster erstellen. Dieser Schritt entspricht [Schritt 3: Konfigurieren der Sicherheit](service-fabric-cluster-creation-via-portal.md#step-3--configure-security) im Clustererstellungsvorgang. Er veranschaulicht, wie Sie die Sicherheitskonfigurationen einrichten.

>[AZURE.NOTE]
Die benötigten Zertifikate werden auf Knotentypebene unter den Sicherheitskonfigurationen angegeben. Sie müssen diese für jeden Knotentyp in Ihrem Cluster angeben. In diesem Dokument wird die Vorgehensweise im Portal erläutert. Sie können jedoch auch eine Azure-Ressourcen-Manager-Vorlage verwenden.

![Screenshot der Sicherheitskonfigurationen im Azure-Portal][SecurityConfigurations_01]

### Erforderliche Parameter

- **Sicherheitsmodus:** Wählen Sie **X509-Zertifikat**, um einen mit X.509-Zertifikaten gesicherten Cluster einzurichten.
- **Clusterschutzebene:** Informationen zur Bedeutung der einzelnen Werte finden Sie im Dokument [Grundlagen der Schutzebene](https://msdn.microsoft.com/library/aa347692.aspx). Obwohl hier drei Werte („EncryptAndSign“, „Sign“ und „None“) zulässig sind, empfiehlt es sich, den Standardwert „EncryptAndSign“ beizubehalten. Sie sollten diesen Wert nur ändern, wenn Sie mit dem Vorgang vertraut sind.
- **Quelltresor:** Dies bezieht sich auf die Ressourcen-ID des Schlüsseltresors. Sie sollte folgendes Format aufweisen:

    ```
    /subscriptions/<Sub ID>/resourceGroups/<Resource group name>/providers/Microsoft.KeyVault/vaults/<vault name>
    ```

- **Zertifikat-URL:** Dies bezieht sich auf die Speicherort-URL in Ihrem Schlüsseltresor, in den das Zertifikat hochgeladen wurde. Sie sollte folgendes Format aufweisen:

    ```
    https://<name of the vault>.vault.azure.net:443/secrets/<exact location>
```
```
    https://chackdan-kmstest-eastus.vault.azure.net:443/secrets/MyCert/6b5cc15a753644e6835cb3g3486b3812
    ```

- **Zertifikatfingerabdruck:** Dies bezieht sich auf den Fingerabdruck des Zertifikats, der unter der zuvor festgelegten URL zu finden ist.

### Optionale Parameter

 Sie können optional weitere Zertifikate für Clientcomputer angeben, die Sie zum Ausführen von Vorgängen im Cluster verwenden. Standardmäßig wird der Fingerabdruck, den Sie in den erforderlichen Parametern angegeben haben, der autorisierten Liste der Fingerabdrücke hinzugefügt, die für Clientvorgänge zugelassen werden.

**Admin-Client:** Mit diesen Informationen wird überprüft, ob der Client für die Verbindung mit dem Endpunkt der Clusterverwaltung die richtigen Anmeldedaten zum Ausführen von Administratorvorgängen sowie schreibgeschützten Vorgängen im Cluster verwendet. Sie können weitere Zertifikate angeben, wenn Sie für Administratorvorgänge autorisiert werden möchten.

- **Autorisieren mit:** Gibt für Service Fabric an, ob dieses Zertifikat anhand des Antragstellernamens oder anhand des Fingerabdrucks gesucht werden soll. Die Verwendung des Antragstellernamens für die Autorisierung ist aus Sicherheitsgründen nicht empfehlenswert, lässt jedoch mehr Flexibilität zu.
- **Antragstellername:** Dieser Name ist nur erforderlich, wenn Sie angegeben haben, dass die Autorisierung mit dem Antragstellernamen erfolgen soll.
- **Fingerabdruck des Ausstellers:** Eine zusätzliche Sicherheitsprüfung, die der Server ausführen kann, wenn ein Client seine Anmeldeinformationen an den Server übermittelt.

**Schreibgeschützter Client:** Mit diesen Informationen wird überprüft, ob der Client für die Verbindung mit dem Endpunkt der Clusterverwaltung die richtigen Anmeldedaten zum Ausführen schreibgeschützter Vorgänge im Cluster verwendet. Sie können weitere Zertifikate angeben, wenn Sie für schreibgeschützte Vorgänge autorisiert werden möchten.

- **Autorisieren mit:** Gibt für Service Fabric an, ob dieses Zertifikat anhand des Antragstellernamens oder anhand des Fingerabdrucks gesucht werden soll. Die Verwendung des Antragstellernamens für die Autorisierung ist aus Sicherheitsgründen nicht empfehlenswert, lässt jedoch mehr Flexibilität zu.
- **Antragstellername:** Dieser Name ist nur erforderlich, wenn Sie angegeben haben, dass die Autorisierung mit dem Antragstellernamen erfolgen soll.
- **Fingerabdruck des Ausstellers:** Eine zusätzliche Sicherheitsprüfung, die der Server ausführen kann, wenn ein Client seine Anmeldeinformationen an den Server übermittelt.

## Nächste Schritte
Nach dem Konfigurieren der Zertifikatsicherheit im Cluster setzen Sie die Clustererstellung mit [Schritt 4: Abschließen der Clustererstellung](service-fabric-cluster-creation-via-portal.md#step-4--complete-the-cluster-creation) fort.

Nach der Erstellung des Clusters mit Zertifikatsicherheit können Sie zu einem späteren Zeitpunkt [ein Zertifikat aktualisieren](service-fabric-cluster-security-update-certs-azure.md).


<!--Image references-->
[SecurityConfigurations_01]: ./media/service-fabric-cluster-azure-secure-with-certs/SecurityConfigurations_01.png
[SecurityConfigurations_02]: ./media/service-fabric-cluster-azure-secure-with-certs/SecurityConfigurations_02.png

<!---HONumber=AcomDC_0608_2016-->