
---
title: Erstellen eines sicheren Service Fabric-Clusters mithilfe von Azure Resource Manager | Microsoft Docs
description: Dieser Artikel beschreibt, wie Sie unter Verwendung von Azure Resource Manager, Azure Key Vault und Azure Active Directory (AAD) für die Clientauthentifizierung einen sicheren Service Fabric-Cluster in Azure einrichten.
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: vturecek

ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/25/2016
ms.author: vturecek

---
# <a name="create-a-service-fabric-cluster-in-azure-using-azure-resource-manager"></a>Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure-Portal](service-fabric-cluster-creation-via-portal.md)
> 
> 

Diese Schrittanleitung führt Sie durch die Einrichtung eines sicheren Azure Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager. In diesem Leitfaden werden die folgenden Schritte behandelt:

* Einrichten von Key Vault zum Verwalten von Schlüsseln für die Sicherheit von Clustern und Anwendungen.
* Erstellen eines gesicherten Clusters in Azure mit Azure Resource Manager.
* Authentifizieren von Benutzern mit Azure Active Directory (AAD) für die Clusterverwaltung.

Ein sicherer Cluster ist ein Cluster, der nicht autorisierten Zugriff auf Verwaltungsvorgänge verhindert, wie z.B. das Bereitstellen, Aktualisieren und Löschen von Anwendungen, Diensten und den darin enthaltenen Daten. Ein unsicherer Cluster ist ein Cluster, mit dem jeder Benutzer jederzeit eine Verbindung herstellen kann, um Verwaltungsvorgänge auszuführen. Die Erstellung eines unsicheren Clusters ist zwar möglich, jedoch **wird dringend empfohlen, einen sicheren Cluster zu erstellen**. Ein unsicherer Cluster **kann nicht zu einem späteren Zeitpunkt gesichert werden**. Stattdessen muss ein neuer Cluster erstellt werden.

Die gleichen Konzepte kommen auch beim Erstellen sicherer Linux- oder Windows-Cluster zur Anwendung. Weitere Informationen und Hilfsskripts zur Erstellung sicherer Linux-Cluster finden Sie unter [Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager](#secure-linux-clusters)

## <a name="log-in-to-azure"></a>Anmelden an Azure
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

## <a name="set-up-key-vault"></a>Einrichten von Key Vault
In diesem Abschnitt erfahren Sie Schritt für Schritt, wie Sie einen Schlüsseltresor für einen Service Fabric-Cluster in Azure und für Service Fabric-Anwendungen erstellen. Eine vollständige Anleitung zu Key Vault finden Sie unter [Erste Schritte mit Key Vault][key-vault-get-started].

Service Fabric verwendet X.509-Zertifikate, um einen Cluster zu sichern und Sicherheitsfunktionen für Anwendungen bereitzustellen. Zertifikate für Service Fabric-Cluster in Azure werden in Azure Key Vault verwaltet. Wenn ein Cluster in Azure bereitgestellt wird, ruft der für die Erstellung von Service Fabric-Clustern zuständige Azure-Ressourcenanbieter Zertifikate aus dem Schlüsseltresor ab und installiert sie auf den virtuellen Clustercomputern.

Das folgende Diagramm veranschaulicht die Beziehung zwischen dem Schlüsseltresor, einem Service Fabric-Cluster und dem Azure-Ressourcenanbieter, der beim Erstellen eines Clusters die in Key Vault gespeicherten Zertifikate verwendet:

![Zertifikatinstallation][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Im ersten Schritt wird speziell für Key Vault eine neue Ressourcengruppe erstellt. Es empfiehlt sich, Key Vault in einer eigenen Ressourcengruppe zu platzieren. Dadurch können Sie die Compute- und Speicherressourcengruppen (einschließlich der Ressourcengruppe mit Ihrem Service Fabric-Cluster) entfernen, ohne Ihre Schlüssel und geheimen Schlüssel zu verlieren. Die Ressourcengruppe, die Ihren Schlüsseltresor enthält, muss sich in der gleichen Region befinden wie der Cluster, der den Tresor verwendet.

```powershell

    New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: The output object type of this cmdlet is going to be modified in a future release.

    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

### <a name="create-key-vault"></a>Erstellen eines Schlüsseltresors
Erstellen Sie einen Schlüsseltresor in der neuen Ressourcengruppe. Der Schlüsseltresor **muss für die Bereitstellung aktiviert sein** , damit der Service Fabric-Ressourcenanbieter Zertifikate daraus abrufen und diese auf Clusterknoten installieren kann:

```powershell

    New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment


    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
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

Wenn Sie bereits über einen Schlüsseltresor verfügen, können Sie diesen über die Azure-Befehlszeilenschnittstelle für die Bereitstellung aktivieren:

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```

<a id="add-certificate-to-key-vault"></a>

## <a name="add-certificates-to-key-vault"></a>Hinzufügen von Zertifikaten zum Schlüsseltresor
Zertifikate werden in Service Fabric zur Authentifizierung und Verschlüsselung verwendet, um verschiedene Aspekte eines Clusters und der zugehörigen Anwendungen zu sichern. Weitere Informationen zur Verwendung von Zertifikaten in Service Fabric finden Sie unter [Szenarios für die Clustersicherheit in Service Fabric][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-(required)"></a>Cluster- und Serverzertifikat (erforderlich)
Dieses Zertifikat wird benötigt, um einen Cluster zu sichern und nicht autorisierte Zugriffe auf den Cluster zu verhindern. Es sorgt auf unterschiedliche Weise für Clustersicherheit:

* **Clusterauthentifizierung** : Authentifiziert die Kommunikation zwischen Knoten für einen Clusterverbund. Nur Knoten, die ihre Identität mit diesem Zertifikat nachweisen können, dürfen dem Cluster beitreten.
* **Serverauthentifizierung** : Authentifiziert die Verwaltungsendpunkte des Clusters bei einem Verwaltungsclient, sodass der Verwaltungsclient weiß, dass die Kommunikation tatsächlich aus dem Cluster stammt. Dieses Zertifikat stellt auch SSL für die HTTPS-Verwaltungs-API und für Service Fabric Explorer über HTTPS bereit.

Für diese Zwecke muss das Zertifikat die folgenden Anforderungen erfüllen:

* Das Zertifikat muss einen privaten Schlüssel enthalten.
* Das Zertifikat muss für den Schlüsselaustausch erstellt werden und in eine PFX-Datei (Persönlicher Informationsaustausch) exportiert werden können.
* Der Name des Antragstellers für das Zertifikat muss der Domäne entsprechen, über die auf den Service Fabric-Cluster zugegriffen wird. Diese Übereinstimmung ist erforderlich, damit SSL für die HTTPS-Verwaltungsendpunkte des Clusters und für Service Fabric Explorer bereitgestellt werden kann. Für die Domäne `.cloudapp.azure.com` können Sie kein SSL-Zertifikat von einer Zertifizierungsstelle beziehen. Sie benötigen einen benutzerdefinierten Domänennamen für Ihren Cluster. Wenn Sie ein Zertifikat von einer Zertifizierungsstelle anfordern, muss der Name des Antragstellers für das Zertifikat dem benutzerdefinierten Domänennamen entsprechen, der für Ihren Cluster verwendet wird.

### <a name="application-certificates-(optional)"></a>Anwendungszertifikate (optional)
Zum Zweck der Anwendungssicherheit kann eine beliebige Anzahl zusätzlicher Zertifikate in einem Cluster installiert werden. Bevor Sie den Cluster erstellen, betrachten Sie die verschiedenen Szenarien zur Anwendungssicherheit, in denen ein Zertifikat auf den Knoten installiert werden muss, beispielsweise:

* Verschlüsselung und Entschlüsselung von Anwendungskonfigurationswerten
* Knotenübergreifende Verschlüsselung von Daten während der Replikation 

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Formatieren von Zertifikaten für die Verwendung durch einen Azure-Ressourcenanbieter
Private Schlüsseldateien (.pfx) können direkt über Key Vault hinzugefügt und verwendet werden. Für den Azure-Ressourcenanbieter müssen Schlüssel jedoch in einem bestimmten JSON-Format gespeichert werden, das die PFX-Datei, eine Base64-codierte Zeichenfolge und das Kennwort für den privaten Schlüssel enthält. Um diese Anforderungen zu erfüllen, müssen die Schlüssel in einer JSON-Zeichenfolge platziert und dann als *geheime Schlüssel* im Schlüsseltresor gespeichert werden.

Um diesen Prozess zu vereinfachen, ist ein PowerShell-Modul [auf GitHub verfügbar][service-fabric-rp-helpers]. Gehen Sie zur Verwendung des Moduls wie folgt vor:

1. Laden Sie den gesamten Inhalt des Repositorys in ein lokales Verzeichnis herunter. 
2. Importieren Sie das Modul in Ihr PowerShell-Fenster:
   
   ```powershell
   PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
   ```

Der Befehl `Invoke-AddCertToKeyVault` in diesem PowerShell-Modul formatiert einen privaten Zertifikatschlüssel automatisch in eine JSON-Zeichenfolge und lädt diese in Key Vault hoch. Verwenden Sie diesen Befehl, um das Clusterzertifikat und weitere zusätzliche Anwendungszertifikate zum Schlüsseltresor hinzuzufügen. Wiederholen Sie diesen Schritt für alle weiteren Zertifikate, die Sie in Ihrem Cluster installieren möchten.

```powershell
 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to myvault in vault myvault


Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

Diese Zeichenfolgen umfassen alle Key Vault-Komponenten, die benötigt werden, um eine Resource Manager-Vorlage für einen Service Fabric-Cluster zu konfigurieren, die Zertifikate für die Knotenauthentifizierung, die Sicherheit und Authentifizierung von Verwaltungsendpunkten sowie weitere Sicherheitsfeatures für Anwendungen installiert, die X.509-Zertifikate verwenden. An diesem Punkt sollten Sie über folgendes Setup in Azure verfügen:

* Schlüsseltresor-Ressourcengruppe
  * Schlüsseltresor
    * Authentifizierungszertifikat für den Clusterserver
    * Anwendungszertifikate

## <a name="set-up-azure-active-directory-for-client-authentication"></a>Einrichten von Azure Active Directory für die Clientauthentifizierung
AAD ermöglicht Organisationen (als Mandanten bezeichnet) die Verwaltung des Benutzerzugriffs auf Anwendungen, die in Anwendungen mit einer webbasierten Anmeldebenutzeroberfläche und Anwendungen mit einer nativen Clientbenutzeroberfläche unterteilt sind. In diesem Dokument wird davon ausgegangen, dass Sie bereits einen Mandanten erstellt haben. Falls nicht, sollten Sie sich zuerst den Artikel [Gewusst wie: Einrichten eines Azure Active Directory-Mandanten][active-directory-howto-tenant] durchlesen.

Service Fabric-Cluster bieten unterschiedliche Einstiegspunkte für ihre Verwaltungsfunktionen. Hierzu zählen etwa der webbasierte [Service Fabric Explorer][service-fabric-visualizing-your-cluster] und [Visual Studio][service-fabric-manage-application-in-visual-studio]. Daher erstellen Sie zwei AAD-Anwendungen, um den Zugriff auf den Cluster zu steuern: eine Webanwendung und eine native Anwendung.

Wir haben einen Satz mit Windows PowerShell-Skripts erstellt, um einige Schritte der Konfiguration von AAD mit einem Service Fabric-Cluster zu vereinfachen.

> [!NOTE]
> Diese Schritte müssen *vor* der Clustererstellung ausgeführt werden. Falls für die Skripts Clusternamen und Endpunkte verwendet werden, sollte es sich dabei also um geplante Werte handeln, nicht um bereits erstellte Werte.
> 
> 

1. [Laden Sie die Skripts][sf-aad-ps-script-download] auf Ihren Computer herunter.
2. Klicken Sie mit der rechten Maustaste auf die ZIP-Datei, wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Zulassen**, und übernehmen Sie die Änderung.
3. Extrahieren Sie die ZIP-Datei.
4. Führen Sie `SetupApplications.ps1`aus, und geben Sie „TenantId“, „ClusterName“ und „WebApplicationReplyUrl“ als Parameter an. Beispiel:
   
    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```
   
    Die **TenantId** können Sie anhand der URL für den Mandanten im klassischen Azure-Portal ermitteln. Die GUID, die in diese URL eingebettet ist, ist die TenantId. Beispiel:
   
    https://<i></i>manage.windowsazure.com/microsoft.onmicrosoft.com#Workspaces/ActiveDirectoryExtension/Directory/**690ec069-8200-4068-9d01-5aaf188e557a**/users
   
    **ClusterName** wird verwendet, um die vom Skript erstellten AAD-Anwendungen mit einem Präfix zu versehen. Dies muss keine exakte Übereinstimmung mit dem tatsächlichen Clusternamen sein. Er dient lediglich als Hilfe, um Ihnen die Zuordnung von AAD-Artefakten zu dem Service Fabric-Cluster zu erleichtern, für den sie verwendet werden.
   
    **WebApplicationReplyUrl** ist der Standardendpunkt, den AAD nach Abschluss des Anmeldeprozesses an Ihre Benutzer zurückgibt. Sie sollten diesen Wert auf den Service Fabric Explorer-Endpunkt für Ihren Cluster festlegen, also standardmäßig:
   
    https://&lt;Cluster-Domäne&gt;:19080/Explorer
   
    Sie werden aufgefordert, sich bei einem Konto anzumelden, das über Administratorrechte für den AAD-Mandanten verfügt. Nach der Anmeldung erstellt das Skript die Webanwendung und die native Anwendung, die Ihren Service Fabric-Cluster bilden. Wenn Sie sich die Anwendungen des Mandanten im [klassischen Azure-Portal][azure-classic-portal] ansehen, sollten zwei neue Einträge angezeigt werden:
   
   * *Clustername*\_Cluster
   * *Clustername*\_Client
     
     Das Skript gibt den für die Azure Resource Manager-Vorlage erforderlichen JSON-Code aus, wenn Sie den Cluster im nächsten Abschnitt erstellen. Lassen Sie das PowerShell-Fenster also geöffnet.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Erstellen einer Resource Manager-Vorlage für Service Fabric-Cluster
In diesem Abschnitt wird die Ausgabe der vorherigen PowerShell-Befehle in einer Resource Manager-Vorlage für Service Fabric-Cluster verwendet.

Resource Manager-Beispielvorlagen stehen im [Azure-Schnellstart-Vorlagenkatalog auf GitHub][azure-quickstart-templates] zur Verfügung. Diese Vorlagen können als Ausgangspunkt für Ihre Clustervorlage verwendet werden. 

### <a name="create-the-resource-manager-template"></a>Erstellen der Resource Manager-Vorlage
In diesem Leitfaden werden die Beispielvorlage für einen [sicheren Cluster mit 5 Knoten][service-fabric-secure-cluster-5-node-1-nodetype-wad] und die zugehörigen Vorlagenparameter verwendet. Laden Sie `azuredeploy.json` und `azuredeploy.parameters.json` auf Ihren Computer herunter, und öffnen Sie beide Dateien in einem Texteditor.

### <a name="add-certificates"></a>Hinzufügen von Zertifikaten
Sie fügen Zertifikate zu einer Resource Manager-Vorlage für einen Cluster hinzu, indem Sie auf den Schlüsseltresor verweisen, der die Zertifikatschlüssel enthält. Es empfiehlt sich, diese Key Vault-Werte in einer Parameterdatei für die Resource Manager-Vorlage zu speichern, damit die Resource Manager-Vorlagendatei wiederverwendet werden kann und keine spezifischen Werte für eine bestimmte Bereitstellung enthält.

#### <a name="add-all-certificates-to-the-vmss-osprofile"></a>Hinzufügen aller Zertifikate zu VMSS osProfile
Jedes Zertifikat, das im Cluster installiert werden soll, muss im osProfile-Abschnitt der VMSS-Ressource (Microsoft.Compute/virtualMachineScaleSets) konfiguriert werden. Dadurch wird der Ressourcenanbieter angewiesen, das Zertifikat auf den virtuellen Computern zu installieren. Dies umfasst das Clusterzertifikat sowie sämtliche Anwendungssicherheitszertifikate, die Sie für Ihre Anwendungen verwenden möchten:

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

#### <a name="configure-service-fabric-cluster-certificate"></a>Konfigurieren des Service Fabric-Clusterzertifikats
Das Zertifikat für die Clusterauthentifizierung muss auch in der Service Fabric-Clusterressource (Microsoft.ServiceFabric/clusters) sowie in der Service Fabric-Erweiterung für VMSS in der VMSS-Ressource konfiguriert werden. Dies ermöglicht dem Service Fabric-Ressourcenanbieter, das Zertifikat für die Clusterauthentifizierung und die Serverauthentifizierung für Verwaltungsendpunkte zu konfigurieren.

##### <a name="vmss-resource:"></a>VMSS-Ressource:
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

##### <a name="service-fabric-resource:"></a>Service Fabric-Ressource:
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

### <a name="insert-aad-config"></a>Einfügen der AAD-Konfiguration
Die zuvor erstellte AAD-Konfiguration kann direkt in Ihre Resource Manager-Vorlage eingefügt werden. Es empfiehlt sich jedoch, vorher die Werte in Parameter in einer Parameterdatei zu extrahieren, damit die Resource Manager-Vorlagendatei wiederverwendet werden kann und keine Werte enthält, die spezifisch für eine bestimmte Bereitstellung gelten.

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

### <a name="<a-"configure-arm"-></a>configure-resource-manager-template-parameters"></a><a "configure-arm" ></a>Konfigurieren von Resource Manager-Vorlagenparametern
Zum Schluss verwenden Sie die Ausgabewerte der Key Vault- und AAD-PowerShell-Befehle, um die Parameterdatei aufzufüllen:

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
An diesem Punkt sollten Sie über Folgendes verfügen:

* Schlüsseltresor-Ressourcengruppe
  * Schlüsseltresor
  * Authentifizierungszertifikat für den Clusterserver
  * Datenverschlüsselungszertifikat
* Azure Active Directory-Mandant 
  * AAD-Anwendung für webbasierte Verwaltung und Service Fabric Explorer
  * AAD-Anwendung für native Clientverwaltung
  * Benutzer mit zugewiesenen Rollen 
* Resource Manager-Vorlage für Service Fabric-Cluster
  * In Key Vault konfigurierte Zertifikate
  * Konfiguriertes Azure Active Directory 

Das folgende Diagramm veranschaulicht die Platzierung der Key Vault- und AAD-Konfiguration in der Resource Manager-Vorlage.

![Abhängigkeiten in Resource Manager][cluster-security-arm-dependency-map]

## <a name="create-the-cluster"></a>Cluster erstellen
Nun können Sie den Cluster mithilfe der [ARM-Bereitstellung][resource-group-template-deploy] erstellen.

#### <a name="test-it"></a>Testen
Verwenden Sie den folgenden PowerShell-Befehl, um Ihre Resource Manager-Vorlage mit einer Parameterdatei zu testen:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a>Bereitstellen
Wenn der Test der Resource Manager-Vorlage erfolgreich verläuft, verwenden Sie den folgenden PowerShell-Befehl, um Ihre Resource Manager-Vorlage mit einer Parameterdatei bereitzustellen:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>

## <a name="assign-users-to-roles"></a>Zuweisen von Benutzern zu Rollen
Nachdem Sie die Anwendungen für Ihren Cluster erstellt haben, müssen Sie Ihre Benutzer den von Service Fabric unterstützten Rollen zuweisen: „read-only“ (schreibgeschützt) und „admin“ (Administrator). Hierfür können Sie das [klassische Azure-Portal][azure-classic-portal] verwenden.

1. Navigieren Sie zu Ihrem Mandanten, und wählen Sie „Anwendungen“.
2. Wählen Sie die Webanwendung aus. Diese besitzt einen Namen wie `myTestCluster_Cluster`.
3. Klicken Sie auf die Registerkarte „Benutzer“.
4. Wählen Sie einen zuzuweisenden Benutzer aus, und klicken Sie am unteren Bildschirmrand auf die Schaltfläche **Zuweisen** .
   
    ![Schaltfläche „Benutzer zu Rollen zuweisen“][assign-users-to-roles-button]
5. Wählen Sie die Rolle aus, die dem Benutzer zugewiesen werden soll.
   
    ![Zuweisen von Benutzern zu Rollen][assign-users-to-roles-dialog]

> [!NOTE]
> Weitere Informationen zu Rollen in Service Fabric finden Sie unter [Rollenbasierte Zugriffssteuerung für Service Fabric-Clients](service-fabric-cluster-security-roles.md).
> 
> 

 <a name="secure-linux-cluster"></a> 

## <a name="create-secure-clusters-on-linux"></a>Erstellen sicherer Cluster unter Linux
Ein Hilfsskript zur Vereinfachung des Prozesses finden Sie [hier](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux). Um dieses Hilfsskript verwenden zu können, muss bereits die Azure-Befehlszeilenschnittstelle installiert sein, und das Skript muss sich in Ihrem Pfad befinden. Vergewissern Sie sich, dass das Skript über Ausführungsberechtigungen verfügt, indem Sie nach dem Herunterladen `chmod +x cert_helper.py` ausführen. Melden Sie sich zunächst über die Befehlszeilenschnittstelle mithilfe des Befehls `azure login` bei Ihrem Azure-Konto an. Verwenden Sie nach der Anmeldung bei Ihrem Azure-Konto das Hilfsprogramm mit Ihrem von der Zertifizierungsstelle signierten Zertifikat, wie im folgenden Befehl zu sehen:

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]

The -ifile parameter can take a .pfx or a .pem file as input, with the certificate type (pfx or pem, or ss if it is a self-signed cert).
The parameter -h prints out the help text.
```

Dieser Befehl gibt als Ausgabe die drei folgenden Zeichenfolgen zurück: 

1. Eine Quelltresor-ID (SourceVaultID): Hierbei handelt es sich um die ID für die neue Key Vault-Ressourcengruppe, die für Sie erstellt wurde. 
2. Eine Zertifikat-URL (CertificateUrl) für den Zugriff auf das Zertifikat.
3. Einen Zertifikatfingerabdruck (CertificateThumbprint) für die Authentifizierung.

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

 Der Name des Antragstellers für das Zertifikat muss der Domäne entsprechen, über die auf den Service Fabric-Cluster zugegriffen wird. Dies ist erforderlich, damit SSL für die HTTPS-Verwaltungsendpunkte des Clusters und für Service Fabric Explorer bereitgestellt werden kann. Für die Domäne `.cloudapp.azure.com` können Sie kein SSL-Zertifikat von einer Zertifizierungsstelle beziehen. Sie benötigen einen benutzerdefinierten Domänennamen für Ihren Cluster. Wenn Sie ein Zertifikat von einer Zertifizierungsstelle anfordern, muss der Name des Antragstellers für das Zertifikat dem benutzerdefinierten Domänennamen entsprechen, der für Ihren Cluster verwendet wird.

Dies sind die Einträge, die Sie zum Erstellen eines sicheren Service Fabric-Clusters (ohne AAD) benötigen (wie unter [Konfigurieren von Resource Manager-Vorlagenparametern](#configure-arm)beschrieben). Eine Anleitung zum Herstellen einer Verbindung mit dem sicheren Cluster finden Sie unter [Herstellen einer Verbindung mit einem sicheren Cluster](service-fabric-connect-to-secure-cluster.md). Linux-Vorschaucluster unterstützen keine AAD-Authentifizierung. Administrator- und Clientrollen können wie im Abschnitt [Zuweisen von Benutzern zu Rollen](#assign-roles)beschrieben zugewiesen werden. Wenn Sie Administrator- und Clientrollen für einen Linux-Vorschaucluster angeben, müssen Sie für die Authentifizierung anstelle des Antragstellernamens Zertifikatfingerabdrücke angeben, da in dieser Vorschauversion keine Kettenüberprüfung oder -sperrung stattfindet.

Wenn Sie zu Testzwecken ein selbstsigniertes Zertifikat verwenden möchten, können Sie mit dem gleichen Skript ein selbstsigniertes Zertifikat generieren und in Key Vault hochladen, indem Sie anstelle von Zertifikatpfad und -name das Flag `ss` angeben. Mit dem folgenden Beispielbefehl wird ein selbstsigniertes Zertifikat erstellt und hochgeladen:

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net" 
```

Dieser Befehl gibt neben den drei Zeichenfolgen („SourceVault“, „CertificateUrl“ und „CertificateThumbprint“), die zur Erstellung eines sicheren Linux-Clusters verwendet werden, auch den Speicherort des selbstsignierten Zertifikats zurück. Sie benötigen das selbstsignierte Zertifikat, um eine Verbindung mit dem Cluster herzustellen.  Eine Anleitung zum Herstellen einer Verbindung mit dem sicheren Cluster finden Sie unter [Herstellen einer Verbindung mit einem sicheren Cluster](service-fabric-connect-to-secure-cluster.md). Der Name des Antragstellers für das Zertifikat muss der Domäne entsprechen, über die auf den Service Fabric-Cluster zugegriffen wird. Dies ist erforderlich, damit SSL für die HTTPS-Verwaltungsendpunkte des Clusters und für Service Fabric Explorer bereitgestellt werden kann. Für die Domäne `.cloudapp.azure.com` können Sie kein SSL-Zertifikat von einer Zertifizierungsstelle beziehen. Sie benötigen einen benutzerdefinierten Domänennamen für Ihren Cluster. Wenn Sie ein Zertifikat von einer Zertifizierungsstelle anfordern, muss der Name des Antragstellers für das Zertifikat dem benutzerdefinierten Domänennamen entsprechen, der für Ihren Cluster verwendet wird.

Die vom Hilfsskript bereitgestellten Parameter können im Portal angegeben werden, wie im Abschnitt [Erstellen eines Clusters im Azure-Portal](service-fabric-cluster-creation-via-portal.md#create-cluster-portal)beschrieben.

## <a name="next-steps"></a>Nächste Schritte
Sie verfügen jetzt über einen sicheren Cluster, für den Azure Active Directory die Authentifizierung für die Verwaltung bereitstellt. Als Nächstes [stellen Sie eine Verbindung mit dem Cluster her](service-fabric-connect-to-secure-cluster.md) und erfahren, wie Sie [Anwendungsgeheimnisse verwalten](service-fabric-application-secret-management.md).

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a>Problembehandlung bei der Einrichtung von Azure Active Directory für die Clientauthentifizierung
Wenn beim Einrichten von Azure Active Directory für die Clientauthentifizierung ein Problem auftritt, finden Sie in den folgenden Vorschlägen mögliche Lösungen.

### <a name="service-fabric-explorer-prompts-for-selecting-certificate"></a>Service Fabric Explorer fordert Sie auf, ein Zertifikat auszuwählen.
#### <a name="problem"></a>Problem
Nach der erfolgreichen Anmeldung auf der AAD-Anmeldeseite in Service Fabric Explorer kehrt der Browser zur Startseite zurück, fordert jedoch in einem Dialogfeld zum Auswählen eines Zertifikats auf.

![SFX-Dialogfeld zum Auswählen eines Zertifikats][sfx-select-certificate-dialog]

#### <a name="reason"></a>Grund
Dem Benutzer wurde keine Rolle in der AAD-Clusteranwendung zugewiesen. Daher tritt bei der AAD-Authentifizierung auf dem Service Fabric-Cluster ein Fehler auf. Service Fabric Explorer greift auf die Zertifikatauthentifizierung zurück.

#### <a name="solution"></a>Lösung
Führen Sie die Schritte zum Einrichten von AAD aus, und weisen Sie Benutzerrollen zu. Darüber hinaus wird empfohlen, „Benutzerzuweisung für den Zugriff auf die App erforderlich“ wie mit `SetupApplications.ps1` zu aktivieren .

### <a name="connect-with-powershell-fails-with-error:-the-specified-credentials-are-invalid"></a>Bei der Verbindung mit PowerShell wird ein Fehler ausgegeben: „Die angegebenen Anmeldeinformationen sind ungültig“.
#### <a name="problem"></a>Problem
Beim Verwenden von PowerShell für die Verbindung mit einem Cluster im Sicherheitsmodus „AzureActiveDirectory“ wird für die Verbindung nach der erfolgreichen Anmeldung auf der AAD-Anmeldeseite folgender Fehler angezeigt: „Die angegebenen Anmeldeinformationen sind ungültig“.

#### <a name="solution"></a>Lösung
Wie oben.

### <a name="service-fabric-explorer-signing-in-return-failure:-aadsts50011"></a>Rückgabe des Fehlers AADSTS50011 bei Anmeldung mit Service Fabric Explorer
#### <a name="problem"></a>Problem
Nach der Anmeldung auf der AAD-Anmeldeseite in Service Fabric Explorer wird der Anmeldefehler AADSTS50011 zurückgegeben. Dieser sagt aus, dass die Antwortadresse &lt;URL&gt; nicht mit den für die Anwendung &lt;GUID&gt; konfigurierten Antwortadressen übereinstimmt. 

![SFX-Antwortadresse stimmt nicht überein.][sfx-reply-address-not-match]

#### <a name="reason"></a>Grund
Die Cluster(web)anwendung für Service Fabric Explorer versucht, sich bei AAD zu authentifizieren, und gibt im Rahmen der Anforderung die Rückgabe-URL für die Umleitung an. Diese ist jedoch in der Liste „ANTWORT-URL“ der AAD-Anwendung nicht aufgeführt.

#### <a name="solution"></a>Lösung
Fügen Sie die URL von Service Fabric Explorer auf der Registerkarte „Konfigurieren“ der Cluster(web)anwendung in die Liste „ANTWORT-URL“ ein, oder ersetzen Sie eines der Elemente in der Liste. Speichern Sie die Liste anschließend.

![Antwort-URL für Webanwendung][web-application-reply-url]

### <a name="can-i-reuse-the-same-aad-tenant-for-multiple-clusters?"></a>Kann ich denselben AAD-Mandanten für mehrere Cluster wiederverwenden?
#### <a name="answer"></a>Antwort
Ja. Denken Sie jedoch daran, die URL von Service Fabric Explorer Ihrer Cluster(web)anwendung hinzuzufügen, da Service Fabric Explorer andernfalls nicht funktioniert.

### <a name="why-do-i-still-need-server-certificate-while-aad-enabled?"></a>Warum benötige ich immer noch Serverzertifikate, wenn AAD aktiviert ist?
#### <a name="answer"></a>Antwort
FabricClient und FabricGateway führen eine wechselseitige Authentifizierung durch. Bei der AAD-Authentifizierung stellt die AAD-Integration die Identität des Clients für den Server bereits, und das Serverzertifikat wird zum Überprüfen der Serveridentität verwendet. Weitere Informationen zur Funktionsweise von Zertifikaten in Service Fabric finden Sie unter [X.509-Zertifikate und Service Fabric][x509-certificates-and-service-fabric].

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
[service-fabric-secure-cluster-5-node-1-nodetype-wad]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype-wad/
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


<!--HONumber=Oct16_HO2-->


