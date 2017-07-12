---
title: Erstellen eines Azure Service Fabric-Clusters auf der Grundlage einer Vorlage| Microsoft-Dokumente
description: "In diesem Artikel erfahren Sie, wie Sie unter Verwendung von Azure Resource Manager, Azure Key Vault und Azure Active Directory (Azure AD) für die Clientauthentifizierung einen sicheren Service Fabric-Cluster in Azure einrichten."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2017
ms.author: chackdan
ms.translationtype: Human Translation
ms.sourcegitcommit: 8d876a0f2168ee9375a3905d5d5a562ab1194cf3
ms.openlocfilehash: 9159f40fed17e52e6576efa1ea7e8a2dee98728e
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017

---
<a id="create-a-service-fabric-cluster-by-using-azure-resource-manager" class="xliff"></a>

# Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure-Portal](service-fabric-cluster-creation-via-portal.md)
>
>

In dieser Schritt-für-Schritt-Anleitung wird die Einrichtung eines sicheren Azure Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager beschrieben. Der Artikel ist recht lang. Dennoch sollten Sie sich genau an die einzelnen Schritte halten, sofern Sie nicht bereits bestens mit dem Inhalt vertraut sind.

In diesem Leitfaden werden folgende Verfahren behandelt:

* Einrichten einer Azure Key Vault-Instanz zum Hochladen von Zertifikaten für die Cluster- und Anwendungssicherheit
* Erstellen eines geschützten Clusters in Azure mit Azure Resource Manager
* Authentifizieren von Benutzern mithilfe von Azure Active Directory (Azure AD) für die Clusterverwaltung

Ein sicherer Cluster verhindert unautorisierte Zugriffe auf Verwaltungsvorgänge. Dazu gehören das Bereitstellen, Aktualisieren und Löschen von Anwendungen, Diensten und der darin enthaltenen Daten. Ein unsicherer Cluster ist ein Cluster, mit dem jeder Benutzer jederzeit eine Verbindung herstellen kann, um Verwaltungsvorgänge auszuführen. Die Erstellung eines unsicheren Clusters ist zwar möglich, es wird jedoch dringend empfohlen, von Anfang an einen sicheren Cluster zu erstellen. Da ein unsicherer Cluster nicht nachträglich in einen sicheren Cluster umgewandelt werden kann, muss ein neuer Cluster erstellt werden.

Das Konzept der Erstellung sicherer Cluster ist für Linux und Windows identisch. Weitere Informationen und Hilfsskripts zur Erstellung sicherer Linux-Cluster finden Sie unter [Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager](#secure-linux-clusters).

<a id="sign-in-to-your-azure-account" class="xliff"></a>

## Anmelden bei Ihrem Azure-Konto
In diesem Leitfaden wird [Azure PowerShell][azure-powershell] verwendet. Wenn Sie eine neue PowerShell-Sitzung starten, melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie Ihr Abonnement aus, bevor Sie Azure-Befehle ausführen.

Melden Sie sich bei Ihrem Azure-Konto an:

```powershell
Login-AzureRmAccount
```

Wählen Sie Ihr Abonnement aus:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

<a id="set-up-a-key-vault" class="xliff"></a>

## Einrichten eines Schlüsseltresors
In diesem Abschnitt erfahren Sie, wie Sie einen Schlüsseltresor für einen Service Fabric-Cluster in Azure und für Service Fabric-Anwendungen erstellen. Eine umfassende Anleitung zu Azure Key Vault finden Sie unter [Erste Schritte mit Key Vault][key-vault-get-started].

Service Fabric verwendet X.509-Zertifikate, um einen Cluster zu sichern und Sicherheitsfunktionen für Anwendungen bereitzustellen. Azure Key Vault dient zum Verwalten von Zertifikaten für Service Fabric-Cluster in Azure. Wenn ein Cluster in Azure bereitgestellt wird, ruft der für die Erstellung von Service Fabric-Clustern zuständige Azure-Ressourcenanbieter Zertifikate aus Key Vault ab und installiert sie auf den virtuellen Clustercomputern.

Das folgende Diagramm veranschaulicht die Beziehung zwischen Azure Key Vault, einem Service Fabric-Cluster und dem Azure-Ressourcenanbieter, der beim Erstellen eines Clusters die in einem Schlüsseltresor gespeicherten Zertifikate verwendet:

![Diagramm für die Zertifikatinstallation][cluster-security-cert-installation]

<a id="create-a-resource-group" class="xliff"></a>

### Erstellen einer Ressourcengruppe
Im ersten Schritt wird speziell für Ihren Schlüsseltresor eine neue Ressourcengruppe erstellt. Es empfiehlt sich, den Schlüsseltresor in einer eigenen Ressourcengruppe zu platzieren. Dadurch können Sie die Compute- und Speicherressourcengruppen (einschließlich der Ressourcengruppe mit Ihrem Service Fabric-Cluster) entfernen, ohne Ihre Schlüssel und geheimen Schlüssel zu verlieren. Die Ressourcengruppe mit Ihrem Schlüsseltresor _muss sich in der gleichen Region befinden_ wie der Cluster, der den Tresor verwendet.

Wenn Sie Cluster in mehreren Regionen bereitstellen möchten, empfiehlt es sich, die Ressourcengruppe und den Schlüsseltresor mit einem Namen zu versehen, der Aufschluss über die jeweilige Regionszugehörigkeit gibt.  

```powershell

    New-AzureRmResourceGroup -Name westus-mykeyvault -Location 'West US'
```
Die Ausgabe sollte wie folgt aussehen:

```powershell

    WARNING: The output object type of this cmdlet is going to be modified in a future release.

    ResourceGroupName : westus-mykeyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/westus-mykeyvault

```
<a id="new-key-vault"></a>

<a id="create-a-key-vault-in-the-new-resource-group" class="xliff"></a>

### Erstellen eines Schlüsseltresors in der neuen Ressourcengruppe
Der Schlüsseltresor _muss für die Bereitstellung aktiviert sein_, damit der Compute-Ressourcenanbieter Zertifikate daraus abrufen und ihn in VM-Instanzen installieren kann:

```powershell

    New-AzureRmKeyVault -VaultName 'mywestusvault' -ResourceGroupName 'westus-mykeyvault' -Location 'West US' -EnabledForDeployment

```

Die Ausgabe sollte wie folgt aussehen:

```powershell

    Vault Name                       : mywestusvault
    Resource Group Name              : westus-mykeyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault
    Vault URI                        : https://mywestusvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all


    Tags                             :
```
<a id="existing-key-vault"></a>

<a id="use-an-existing-key-vault" class="xliff"></a>

## Verwenden eines vorhandenen Schlüsseltresors

Wenn Sie einen bereits vorhandenen Schlüsseltresor verwenden möchten, _muss er für die Bereitstellung aktiviert werden_, damit der Compute-Ressourcenanbieter Zertifikate daraus abrufen und ihn auf Clusterknoten installieren kann:

```powershell

Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

```

<a id="add-certificate-to-key-vault"></a>

<a id="add-certificates-to-your-key-vault" class="xliff"></a>

## Hinzufügen von Zertifikaten zu Ihrem Schlüsseltresor

Zertifikate werden in Service Fabric zur Authentifizierung und Verschlüsselung verwendet, um verschiedene Aspekte eines Clusters und der zugehörigen Anwendungen zu sichern. Weitere Informationen zur Verwendung von Zertifikaten in Service Fabric finden Sie unter [Szenarien für die Clustersicherheit in Service Fabric][service-fabric-cluster-security].

<a id="cluster-and-server-certificate-required" class="xliff"></a>

### Cluster- und Serverzertifikat (erforderlich)
Dieses Zertifikat wird benötigt, um einen Cluster zu sichern und nicht autorisierte Zugriffe auf den Cluster zu verhindern. Es sorgt auf zwei Arten für Clustersicherheit:

* Clusterauthentifizierung: Authentifiziert die Kommunikation zwischen Knoten für einen Clusterverbund. Nur Knoten, die ihre Identität mit diesem Zertifikat nachweisen können, dürfen dem Cluster beitreten.
* Serverauthentifizierung: Authentifiziert die Verwaltungsendpunkte des Clusters für einen Verwaltungsclient, damit der Verwaltungsclient weiß, dass er tatsächlich mit dem Cluster kommuniziert. Dieses Zertifikat stellt auch SSL für die HTTPS-Verwaltungs-API und für Service Fabric Explorer über HTTPS bereit.

Für diese Zwecke muss das Zertifikat die folgenden Anforderungen erfüllen:

* Das Zertifikat muss einen privaten Schlüssel enthalten.
* Das Zertifikat muss für den Schlüsselaustausch erstellt werden, um in eine PFX-Datei (Personal Information Exchange; privater Informationsaustausch) exportiert werden zu können.
* Der Name des Antragstellers für das Zertifikat muss der Domäne entsprechen, über die Sie auf den Service Fabric-Cluster zugreifen. Dies ist erforderlich, damit SSL für die HTTPS-Verwaltungsendpunkte des Clusters und für Service Fabric Explorer bereitgestellt werden kann. Für die Domäne „.cloudapp.azure.com“ können Sie kein SSL-Zertifikat von einer Zertifizierungsstelle beziehen. Sie benötigen einen benutzerdefinierten Domänennamen für Ihren Cluster. Wenn Sie ein Zertifikat von einer Zertifizierungsstelle anfordern, muss der Name des Antragstellers für das Zertifikat dem benutzerdefinierten Domänennamen entsprechen, den Sie für Ihren Cluster verwenden.

<a id="application-certificates-optional" class="xliff"></a>

### Anwendungszertifikate (optional)
Zum Zweck der Anwendungssicherheit kann eine beliebige Anzahl zusätzlicher Zertifikate in einem Cluster installiert werden. Bevor Sie den Cluster erstellen, betrachten Sie die verschiedenen Szenarien zur Anwendungssicherheit, in denen ein Zertifikat auf den Knoten installiert werden muss, beispielsweise:

* Verschlüsselung und Entschlüsselung von Anwendungskonfigurationswerten
* Knotenübergreifende Verschlüsselung von Daten während der Replikation

<a id="formatting-certificates-for-azure-resource-provider-use" class="xliff"></a>

### Formatieren von Zertifikaten für die Verwendung durch einen Azure-Ressourcenanbieter
Sie können private Schlüsseldateien (PFX-Dateien) direkt über Ihren Schlüsseltresor hinzufügen und verwenden. Für den Compute-Ressourcenanbieter müssen Schlüssel jedoch in einem bestimmten JSON-Format (JavaScript Object Notation) gespeichert werden. Dieses Format enthält die PFX-Datei als Base64-codierte Zeichenfolge und das Kennwort für den privaten Schlüssel. Hierzu müssen die Schlüssel in einer JSON-Zeichenfolge platziert und dann als Geheimnisse im Schlüsseltresor gespeichert werden.

Zur Vereinfachung dieses Prozesses ist ein [PowerShell-Modul auf GitHub][service-fabric-rp-helpers] verfügbar. Gehen Sie zum Verwenden dieses Moduls wie folgt vor:

1. Laden Sie den gesamten Inhalt des Repositorys in ein lokales Verzeichnis herunter.
2. Navigieren Sie zu dem lokalen Verzeichnis.
2. Importieren Sie das Modul „ServiceFabricRPHelpers“ über Ihr PowerShell-Fenster:

```powershell

 Import-Module "C:\..\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

```

Der Befehl `Invoke-AddCertToKeyVault` in diesem PowerShell-Modul formatiert einen privaten Zertifikatschlüssel automatisch in eine JSON-Zeichenfolge und lädt sie in den Schlüsseltresor hoch. Verwenden Sie den Befehl, um dem Schlüsseltresor das Clusterzertifikat und ggf. weitere Anwendungszertifikate hinzuzufügen. Wiederholen Sie diesen Schritt für alle weiteren Zertifikate, die Sie in Ihrem Cluster installieren möchten.

<a id="uploading-an-existing-certificate" class="xliff"></a>

#### Hochladen eines vorhandenen Zertifikats

```powershell

 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName westus-mykeyvault -Location "West US" -VaultName mywestusvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

```

Fehler wie der hier gezeigte sind in der Regel auf einen URL-Ressourcenkonflikt zurückzuführen. Ändern Sie zum Beheben des Konflikts den Namen des Schlüsseltresors.

```
Set-AzureKeyVaultSecret : The remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

Nach Behebung des Konflikts sollte die Ausgabe wie folgt aussehen:

```

    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup westus-mykeyvault in West US
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    Using existing value mywestusvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to mywestusvault in vault mywestusvault


Name  : CertificateThumbprint
Value : E21DBC64B183B5BF355C34C46E03409FEEAEF58D

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault

Name  : CertificateURL
Value : https://mywestusvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

>[!NOTE]
>Sie benötigen die drei vorherigen Zeichenfolgen „CertificateThumbprint“, „SourceVault“ und „CertificateURL“, um einen sicheren Service Fabric-Cluster einzurichten und die Anwendungszertifikate abzurufen, die Sie ggf. für die Anwendungssicherheit verwenden. Wenn die Zeichenfolgen nicht gespeichert werden, können sie später unter Umständen nur sehr umständlich durch Abfragen des Schlüsseltresors abgerufen werden.

<a id="add-self-signed-certificate-to-key-vault"></a>

<a id="creating-a-self-signed-certificate-and-uploading-it-to-the-key-vault" class="xliff"></a>

#### Erstellen eines selbstsignierten Zertifikats und Hochladen des Zertifikats in den Schlüsseltresor

Falls Sie Ihre Zertifikate bereits in den Schlüsseltresor hochgeladen haben, überspringen Sie diesen Schritt. In diesem Schritt wird ein neues selbstsigniertes Zertifikat generiert und in Ihren Schlüsseltresor hochgeladen. Nachdem Sie die Parameter im folgenden Skript geändert und das Skript ausgeführt haben, sollten Sie zur Eingabe eines Zertifikatkennworts aufgefordert werden.  

```powershell

$ResourceGroup = "chackowestuskv"
$VName = "chackokv2"
$SubID = "6c653126-e4ba-42cd-a1dd-f7bf96ae7a47"
$locationRegion = "westus"
$newCertName = "chackotestcertificate1"
$dnsName = "www.mycluster.westus.mydomain.com" #The certificate's subject name must match the domain used to access the Service Fabric cluster.
$localCertPath = "C:\MyCertificates" # location where you want the .PFX to be stored

 Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResourceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath

```

Fehler wie der hier gezeigte sind in der Regel auf einen URL-Ressourcenkonflikt zurückzuführen. Ändern Sie zum Beheben des Konflikts den Namen des Schlüsseltresors, den Namen der Ressourcengruppe usw.

```
Set-AzureKeyVaultSecret : The remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

Nach Behebung des Konflikts sollte die Ausgabe wie folgt aussehen:

```
PS C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers> Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResouceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -Password $certPassword -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath
Switching context to SubscriptionId 6c343126-e4ba-52cd-a1dd-f8bf96ae7a47
Ensuring ResourceGroup chackowestuskv in westus
WARNING: The output object type of this cmdlet will be modified in a future release.
Creating new vault westuskv1 in westus
Creating new self signed certificate at C:\MyCertificates\chackonewcertificate1.pfx
Reading pfx file from C:\MyCertificates\chackonewcertificate1.pfx
Writing secret to chackonewcertificate1 in vault westuskv1


Name  : CertificateThumbprint
Value : 96BB3CC234F9D43C25D4B547sd8DE7B569F413EE

Name  : SourceVault
Value : /subscriptions/6c653126-e4ba-52cd-a1dd-f8bf96ae7a47/resourceGroups/chackowestuskv/providers/Microsoft.KeyVault/vaults/westuskv1

Name  : CertificateURL
Value : https://westuskv1.vault.azure.net:443/secrets/chackonewcertificate1/ee247291e45d405b8c8bbf81782d12bd

```

>[!NOTE]
>Sie benötigen die drei vorherigen Zeichenfolgen „CertificateThumbprint“, „SourceVault“ und „CertificateURL“, um einen sicheren Service Fabric-Cluster einzurichten und die Anwendungszertifikate abzurufen, die Sie ggf. für die Anwendungssicherheit verwenden. Wenn die Zeichenfolgen nicht gespeichert werden, können sie später unter Umständen nur sehr umständlich durch Abfragen des Schlüsseltresors abgerufen werden.

 Sie müssten nun über folgende Elemente verfügen:

* Schlüsseltresor-Ressourcengruppe
* Schlüsseltresor und die dazugehörige URL („SourceVault“ in der vorherigen PowerShell-Ausgabe)
* Authentifizierungszertifikat für den Clusterserver und die dazugehörige URL im Schlüsseltresor
* Anwendungszertifikate und die dazugehörigen URLs im Schlüsseltresor


<a id="add-AAD-for-client"></a>

<a id="set-up-azure-active-directory-for-client-authentication" class="xliff"></a>

## Einrichten von Azure Active Directory für die Clientauthentifizierung

Mit Azure AD können Organisationen (so genannte Mandanten) den Benutzerzugriff auf Anwendungen verwalten. Bei den Anwendungen wird zwischen Anwendungen mit webbasierter Anmeldebenutzeroberfläche und Anwendungen mit nativer Clientumgebung unterschieden. In diesem Artikel wird davon ausgegangen, dass Sie bereits einen Mandanten erstellt haben. Falls nicht, sollten Sie sich zunächst mit dem Artikel [Einrichten eines Azure Active Directory-Mandanten][active-directory-howto-tenant] befassen.

Service Fabric-Cluster bieten unterschiedliche Einstiegspunkte für ihre Verwaltungsfunktionen. Hierzu zählen etwa der webbasierte [Service Fabric Explorer][service-fabric-visualizing-your-cluster] und [Visual Studio][service-fabric-manage-application-in-visual-studio]. Daher erstellen Sie zwei Azure AD-Anwendungen, um den Zugriff auf den Cluster zu steuern: eine Webanwendung und eine native Anwendung.

Wir haben eine Reihe von Windows PowerShell-Skripts erstellt, um einige Schritte der Konfiguration von Azure AD mit einem Service Fabric-Cluster zu vereinfachen.

> [!NOTE]
> Vor der Clustererstellung müssen folgende Schritte ausgeführt werden. Da in den Skripts Clusternamen und Endpunkte erwartet werden, sollte es sich bei den Werten nicht um bereits erstellte, sondern um geplante Werte handeln.

1. [Laden Sie die Skripts auf Ihren Computer herunter][sf-aad-ps-script-download].
2. Klicken Sie mit der rechten Maustaste auf die ZIP-Datei, wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Entsperren**, und klicken Sie anschließend auf **Übernehmen**.
3. Extrahieren Sie die ZIP-Datei.
4. Führen Sie `SetupApplications.ps1` aus, und geben Sie „TenantId“, „ClusterName“ und „WebApplicationReplyUrl“ als Parameter an. Beispiel:

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    Die Mandanten-ID (TenantId) können Sie mithilfe des PowerShell-Befehls `Get-AzureSubscription` ermitteln. Wenn Sie diesen Befehl ausführen, wird die Mandanten-ID für jedes Abonnement angezeigt.

    „ClusterName“ wird als Präfix für durch das Skript erstellte Azure AD-Anwendungen verwendet. Der Wert muss nicht exakt dem tatsächlichen Clusternamen entsprechen. Er dient lediglich zur Vereinfachung der Zuordnung von Azure AD-Artefakten zu dem Service Fabric-Cluster, mit dem sie verwendet werden.

    „WebApplicationReplyUrl“ ist der Standardendpunkt, den Azure AD nach Abschluss des Anmeldeprozesses an Ihre Benutzer zurückgibt. Legen Sie diesen Endpunkt auf den Service Fabric Explorer-Endpunkt für Ihren Cluster fest. Dies ist standardmäßig folgender Wert:

    https://&lt;Cluster-Domäne&gt;:19080/Explorer

    Sie werden aufgefordert, sich bei einem Konto anzumelden, das über Administratorrechte für den Azure AD-Mandanten verfügt. Nach der Anmeldung erstellt das Skript die Webanwendung und die native Anwendung, die Ihren Service Fabric-Cluster darstellen. Wenn Sie sich die Anwendungen des Mandanten im [klassischen Azure-Portal][azure-classic-portal] ansehen, sollten zwei neue Einträge angezeigt werden:

   * *Clustername*\_Cluster
   * *Clustername*\_Client

   Das Skript gibt den für die Azure Resource Manager-Vorlage erforderlichen JSON-Code aus, wenn Sie im nächsten Abschnitt den Cluster erstellen. Es empfiehlt sich daher, das PowerShell-Fenster geöffnet zu lassen.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

<a id="create-a-service-fabric-cluster-resource-manager-template" class="xliff"></a>

## Erstellen einer Resource Manager-Vorlage für Service Fabric-Cluster
In diesem Abschnitt werden die Ausgaben der vorherigen PowerShell-Befehle in einer Resource Manager-Vorlage für Service Fabric-Cluster verwendet.

Resource Manager-Beispielvorlagen stehen im [Azure-Schnellstart-Vorlagenkatalog auf GitHub][azure-quickstart-templates] zur Verfügung. Diese Vorlagen können als Ausgangspunkt für Ihre Clustervorlage verwendet werden.

<a id="create-the-resource-manager-template" class="xliff"></a>

### Erstellen der Resource Manager-Vorlage
In diesem Leitfaden werden die Beispielvorlage für einen [Sicherheitscluster mit 5 Knoten][service-fabric-secure-cluster-5-node-1-nodetype] und die zugehörigen Vorlagenparameter verwendet. Laden Sie `azuredeploy.json` und `azuredeploy.parameters.json` auf Ihren Computer herunter, und öffnen Sie beide Dateien in einem Texteditor.

<a id="add-certificates" class="xliff"></a>

### Hinzufügen von Zertifikaten
Um Zertifikate einer Resource Manager-Vorlage für einen Cluster hinzuzufügen, verweisen Sie auf den Schlüsseltresor mit den Zertifikatschlüsseln. Es empfiehlt sich, die Schlüsseltresorwerte in einer Parameterdatei für die Resource Manager-Vorlage zu speichern. Dadurch ist die Resource Manager-Vorlagendatei wiederverwendbar und enthält keine spezifischen Werte für eine bestimmte Bereitstellung.

<a id="add-all-certificates-to-the-virtual-machine-scale-set-osprofile" class="xliff"></a>

#### Hinzufügen aller Zertifikate zur VM-Skalierungsgruppe „osProfile“
Jedes im Cluster installierte Zertifikat muss im osProfile-Abschnitt der Skalierungsgruppenressource (Microsoft.Compute/virtualMachineScaleSets) konfiguriert werden. Dadurch wird der Ressourcenanbieter angewiesen, das Zertifikat auf den virtuellen Computern zu installieren. Die Installation umfasst sowohl das Clusterzertifikat als auch sämtliche Anwendungssicherheitszertifikate, die Sie für Ihre Anwendungen verwenden möchten:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

<a id="configure-the-service-fabric-cluster-certificate" class="xliff"></a>

#### Konfigurieren des Service Fabric-Clusterzertifikats
Das Zertifikat für die Clusterauthentifizierung muss sowohl in der Service Fabric-Clusterressource (Microsoft.ServiceFabric/clusters) als auch in der Service Fabric-Erweiterung für VM-Skalierungsgruppen in der VM-Skalierungsgruppenressource konfiguriert werden. Dadurch kann der Service Fabric-Ressourcenanbieter das Zertifikat für die Clusterauthentifizierung und die Serverauthentifizierung für Verwaltungsendpunkte konfigurieren.

<a id="virtual-machine-scale-set-resource" class="xliff"></a>

##### VM-Skalierungsgruppenressource:
```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

<a id="service-fabric-resource" class="xliff"></a>

##### Service Fabric-Ressource:
```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

<a id="insert-azure-ad-configuration" class="xliff"></a>

### Einfügen der Azure AD-Konfiguration
Die zuvor erstellte Azure AD-Konfiguration kann direkt in die Resource Manager-Vorlage eingefügt werden. Es empfiehlt sich allerdings, zuerst die Werte in eine Parameterdatei zu extrahieren, damit die Resource Manager-Vorlagendatei wiederverwendet werden kann und keine spezifischen Werte für eine bestimmte Bereitstellung enthält.

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### <a "configure-arm" ></a>Konfigurieren von Resource Manager-Vorlagenparametern
Füllen Sie die Parameterdatei abschließend mit den Ausgabewerten aus den Schlüsseltresor- und Azure AD-PowerShell-Befehlen auf:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```
Sie müssten nun über folgende Elemente verfügen:

* Schlüsseltresor-Ressourcengruppe
  * Schlüsseltresor
  * Authentifizierungszertifikat für den Clusterserver
  * Datenverschlüsselungszertifikat
* Azure Active Directory-Mandant
  * Azure AD-Anwendung für webbasierte Verwaltung und Service Fabric Explorer
  * Azure AD-Anwendung für native Clientverwaltung
  * Benutzer und ihre zugewiesenen Rollen
* Resource Manager-Vorlage für Service Fabric-Cluster
  * Über den Schlüsseltresor konfigurierte Zertifikate
  * Konfiguriertes Azure Active Directory

Das folgende Diagramm veranschaulicht die Platzierung der Schlüsseltresor- und Azure AD-Konfiguration in der Resource Manager-Vorlage.

![Abhängigkeiten in Resource Manager][cluster-security-arm-dependency-map]

<a id="create-the-cluster" class="xliff"></a>

## Cluster erstellen
Nun können Sie den Cluster mithilfe der [Azure-Ressourcenvorlagenbereitstellung][resource-group-template-deploy] erstellen.

<a id="test-it" class="xliff"></a>

#### Testen
Verwenden Sie den folgenden PowerShell-Befehl, um Ihre Resource Manager-Vorlage mit einer Parameterdatei zu testen:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a id="deploy-it" class="xliff"></a>

#### Bereitstellen
Wenn der Test der Resource Manager-Vorlage erfolgreich verläuft, verwenden Sie den folgenden PowerShell-Befehl, um Ihre Resource Manager-Vorlage mit einer Parameterdatei bereitzustellen:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>

<a id="assign-users-to-roles" class="xliff"></a>

## Zuweisen von Benutzern zu Rollen
Nachdem Sie die Anwendungen für Ihren Cluster erstellt haben, müssen Ihre Benutzer den von Service Fabric unterstützten Rollen zugewiesen werden: „read-only“ (schreibgeschützt) und „admin“ (Administrator). Für die Rollenzuweisung können Sie das [klassische Azure-Portal][azure-classic-portal] verwenden.

1. Navigieren Sie im Azure-Portal zu Ihrem Mandanten, und wählen Sie **Anwendungen** aus.
2. Wählen Sie die Webanwendung aus. Diese besitzt einen Namen wie `myTestCluster_Cluster`.
3. Klicken Sie auf die Registerkarte **Benutzer** .
4. Wählen Sie einen zuzuweisenden Benutzer aus, und klicken Sie anschließend am unteren Bildschirmrand auf die Schaltfläche **Zuweisen**.

    ![Schaltfläche „Benutzer zu Rollen zuweisen“][assign-users-to-roles-button]
5. Wählen Sie die Rolle aus, die dem Benutzer zugewiesen werden soll.

    ![Dialogfeld „Benutzer zuweisen“][assign-users-to-roles-dialog]

> [!NOTE]
> Weitere Informationen zu Rollen in Service Fabric finden Sie unter [Rollenbasierte Zugriffssteuerung für Service Fabric-Clients](service-fabric-cluster-security-roles.md).
>
>

 <a name="secure-linux-cluster"></a>

<a id="create-secure-clusters-on-linux" class="xliff"></a>

## Erstellen sicherer Cluster unter Linux
Zur Vereinfachung des Prozesses haben wir ein [Hilfsskript](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux) erstellt. Vergewissern Sie sich vor der Verwendung dieses Hilfsskripts, dass die Azure-Befehlszeilenschnittstelle bereits installiert ist und sich unter Ihrem Pfad befindet. Vergewissern Sie sich, dass das Skript über Ausführungsberechtigungen verfügt, indem Sie nach dem Herunterladen `chmod +x cert_helper.py` ausführen. Melden Sie sich zunächst über die Befehlszeilenschnittstelle mithilfe des Befehls `azure login` bei Ihrem Azure-Konto an. Verwenden Sie nach der Anmeldung bei Ihrem Azure-Konto das Hilfsskript mit Ihrem von der Zertifizierungsstelle signierten Zertifikat, wie im folgenden Befehl zu sehen:

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]
```

Der Parameter „-ifile“ akzeptiert als Eingabe eine PFX- oder eine PEM-Datei mit dem Zertifikattyp („pfx“ oder „pem“ – oder „ss“, falls es sich um ein selbstsigniertes Zertifikat handelt).
Der Parameter „-h“ gibt den Hilfetext aus.


Dieser Befehl gibt als Ausgabe die drei folgenden Zeichenfolgen zurück:

* SourceVaultID: Die ID für die neue Key Vault-Ressourcengruppe, die für Sie erstellt wurde.
* CertificateUrl: Die Zertifikat-URL für den Zugriff auf das Zertifikat.
* CertificateThumbprint: Der Zertifikatfingerabdruck für die Authentifizierung.

Das folgende Beispiel zeigt die Verwendung des Befehls:

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
Wenn Sie diesen Befehl ausführen, erhalten Sie die drei folgenden Zeichenfolgen:

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

Der Name des Antragstellers für das Zertifikat muss der Domäne entsprechen, über die Sie auf den Service Fabric-Cluster zugreifen. Dies ist erforderlich, damit SSL für die HTTPS-Verwaltungsendpunkte des Clusters und für Service Fabric Explorer bereitgestellt werden kann. Für die Domäne `.cloudapp.azure.com` können Sie kein SSL-Zertifikat von einer Zertifizierungsstelle beziehen. Sie benötigen einen benutzerdefinierten Domänennamen für Ihren Cluster. Wenn Sie ein Zertifikat von einer Zertifizierungsstelle anfordern, muss der Name des Antragstellers für das Zertifikat dem benutzerdefinierten Domänennamen entsprechen, den Sie für Ihren Cluster verwenden.

Diese Antragstellernamen sind die Einträge, die Sie zum Erstellen eines sicheren Service Fabric-Clusters (ohne Azure AD) benötigen, wie unter [Konfigurieren von Resource Manager-Vorlagenparametern](#configure-arm)beschrieben. Eine Anleitung zum Herstellen einer Verbindung mit dem sicheren Cluster finden Sie unter [Herstellen einer Verbindung mit einem sicheren Cluster](service-fabric-connect-to-secure-cluster.md). Linux-Vorschaucluster unterstützen keine Azure AD-Authentifizierung. Administrator- und Clientrollen können wie im Abschnitt [Zuweisen von Benutzern zu Rollen](#assign-roles) beschrieben zugewiesen werden. Bei der Angabe von Administrator- und Clientrollen für einen Linux-Vorschaucluster müssen Zertifikatfingerabdrücke für die Authentifizierung angegeben werden. (Der Name des Antragstellers wird nicht angegeben, da in dieser Vorschauversion keine Überprüfung oder Sperrung der Zertifikatkette stattfindet.)

Falls Sie zu Testzwecken ein selbstsigniertes Zertifikat verwenden möchten, können Sie mithilfe des gleichen Skripts eines generieren. Dieses können Sie dann in Ihren Schlüsseltresor hochladen, indem Sie anstelle von Zertifikatpfad und -name das Flag `ss` angeben. Mit dem folgenden Beispielbefehl wird ein selbstsigniertes Zertifikat erstellt und hochgeladen:

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net"
```
Dieser Befehl gibt die gleichen drei Zeichenfolgen („SourceVault“, „CertificateUrl“ und „CertificateThumbprint“) zurück. Mithilfe dieser Zeichenfolgen können Sie dann sowohl einen sicheren Linux-Cluster als auch einen Speicherort für das selbstsignierte Zertifikat erstellen. Sie benötigen das selbstsignierte Zertifikat, um eine Verbindung mit dem Cluster herstellen zu können. Eine Anleitung zum Herstellen einer Verbindung mit dem sicheren Cluster finden Sie unter [Herstellen einer Verbindung mit einem sicheren Cluster](service-fabric-connect-to-secure-cluster.md).

Der Name des Antragstellers für das Zertifikat muss der Domäne entsprechen, über die Sie auf den Service Fabric-Cluster zugreifen. Dies ist erforderlich, damit SSL für die HTTPS-Verwaltungsendpunkte des Clusters und für Service Fabric Explorer bereitgestellt werden kann. Für die Domäne `.cloudapp.azure.com` können Sie kein SSL-Zertifikat von einer Zertifizierungsstelle beziehen. Sie benötigen einen benutzerdefinierten Domänennamen für Ihren Cluster. Wenn Sie ein Zertifikat von einer Zertifizierungsstelle anfordern, muss der Name des Antragstellers für das Zertifikat dem benutzerdefinierten Domänennamen entsprechen, den Sie für Ihren Cluster verwenden.

Die Parameter des Hilfsskripts können im Azure-Portal angegeben werden, wie im Abschnitt [Erstellen eines Clusters im Azure-Portal](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal) beschrieben.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
Sie verfügen jetzt über einen sicheren Cluster, für den Azure Active Directory die Authentifizierung für die Verwaltung bereitstellt. Als Nächstes [stellen Sie eine Verbindung mit dem Cluster her](service-fabric-connect-to-secure-cluster.md) und erfahren, wie Sie [Anwendungsgeheimnisse verwalten](service-fabric-application-secret-management.md).

<a id="troubleshoot-setting-up-azure-active-directory-for-client-authentication" class="xliff"></a>

## Problembehandlung bei der Einrichtung von Azure Active Directory für die Clientauthentifizierung
Sollte beim Einrichten von Azure AD für die Clientauthentifizierung ein Problem auftreten, helfen Ihnen unter Umständen die Lösungsvorschläge in diesem Abschnitt weiter.

<a id="service-fabric-explorer-prompts-you-to-select-a-certificate" class="xliff"></a>

### Service Fabric Explorer fordert Sie auf, ein Zertifikat auszuwählen.
<a id="problem" class="xliff"></a>

#### Problem
Nach erfolgreicher Anmeldung bei Azure AD über Service Fabric Explorer zeigt der Browser wieder die Startseite an und fordert Sie auf, ein Zertifikat auszuwählen.

![SFX-Dialogfeld zum Auswählen eines Zertifikats][sfx-select-certificate-dialog]

<a id="reason" class="xliff"></a>

#### Grund
Dem Benutzer ist in der Azure AD-Clusteranwendung keine Rolle zugewiesen. Daher tritt bei der Azure AD-Authentifizierung im Service Fabric-Cluster ein Fehler auf. Service Fabric Explorer greift auf die Zertifikatauthentifizierung zurück.

<a id="solution" class="xliff"></a>

#### Lösung
Führen Sie die Schritte zum Einrichten von Azure AD aus, und weisen Sie Benutzerrollen zu. Darüber hinaus empfiehlt es sich, „Benutzerzuweisung für den Zugriff auf die App erforderlich“ zu aktivieren, wie dies bei `SetupApplications.ps1` der Fall ist.

<a id="connection-with-powershell-fails-with-an-error-the-specified-credentials-are-invalid" class="xliff"></a>

### Beim Herstellen der Verbindung mit PowerShell tritt der Fehler „Die angegebenen Anmeldeinformationen sind ungültig.“ auf.
<a id="problem" class="xliff"></a>

#### Problem
Wenn Sie die Verbindung mit dem Cluster über PowerShell und unter Verwendung des Sicherheitsmodus „AzureActiveDirectory“ herstellen, tritt nach erfolgreicher Anmeldung bei Azure AD folgender Verbindungsfehler auf: „Die angegebenen Anmeldeinformationen sind ungültig.“

<a id="solution" class="xliff"></a>

#### Lösung
Siehe vorherige Lösung.

<a id="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011" class="xliff"></a>

### Service Fabric Explorer gibt bei der Anmeldung den Fehler „AADSTS50011“ zurück.
<a id="problem" class="xliff"></a>

#### Problem
Wenn Sie versuchen, sich in Service Fabric Explorer bei Azure AD anzumelden, wird der Fehler „AADSTS50011“ mit dem Hinweis zurückgegeben, dass die Antwortadresse &lt;URL&gt; nicht den für die Anwendung &lt;GUID&gt; konfigurierten Antwortadressen entspricht.

![Die SFX-Antwortadresse stimmt nicht überein.][sfx-reply-address-not-match]

<a id="reason" class="xliff"></a>

#### Grund
Die Cluster(web)anwendung für Service Fabric Explorer versucht, sich bei Azure AD zu authentifizieren, und gibt im Rahmen der Anforderung die Rückgabe-URL für die Umleitung an. Die URL ist jedoch nicht in der Liste **ANTWORT-URL** der Azure AD-Anwendung enthalten.

<a id="solution" class="xliff"></a>

#### Lösung
Fügen Sie auf der Registerkarte **Konfigurieren** der Cluster(web)anwendung die URL von Service Fabric Explorer der Liste **ANTWORT-URL** hinzu, oder ersetzen Sie eines der Elemente in der Liste. Speichern Sie anschließend Ihre Änderung.

![Antwort-URL für Webanwendung][web-application-reply-url]

<a id="connect-the-cluster-by-using-azure-ad-authentication-via-powershell" class="xliff"></a>

### Herstellen der Clusterverbindung unter Verwendung der Azure AD-Authentifizierung über PowerShell
Verwenden Sie das folgende PowerShell-Befehlsbeispiel, um eine Service Fabric-Clusterverbindung herzustellen:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

Informationen zum Connect-ServiceFabricCluster-Cmdlet finden Sie unter [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx).

<a id="can-i-reuse-the-same-azure-ad-tenant-in-multiple-clusters" class="xliff"></a>

### Kann ich den gleichen Azure AD-Mandanten in mehreren Clustern verwenden?
Ja. Denken Sie jedoch daran, die URL von Service Fabric Explorer Ihrer Cluster(web)anwendung hinzuzufügen. Andernfalls funktioniert Service Fabric Explorer nicht.

<a id="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled" class="xliff"></a>

### Warum benötige ich ein Serverzertifikat, auch wenn Azure AD aktiviert ist?
„FabricClient“ und „FabricGateway“ führen eine wechselseitige Authentifizierung durch. Bei der Azure AD-Authentifizierung stellt die Azure AD-Integration die Identität des Clients für den Server bereit, und das Serverzertifikat dient zum Überprüfen der Serveridentität. Weitere Informationen zu Service Fabric-Zertifikaten finden Sie unter [X.509-Zertifikate und Service Fabric][x509-certificates-and-service-fabric].

<!-- Links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png


