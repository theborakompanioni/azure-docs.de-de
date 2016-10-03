
<properties
   pageTitle="Erstellen eines sicheren Service Fabric-Clusters mithilfe von Azure Resource Manager | Microsoft Azure"
   description="Dieser Artikel beschreibt, wie Sie unter Verwendung von Azure Resource Manager, Azure Key Vault und Azure Active Directory (AAD) für die Clientauthentifizierung einen sicheren Service Fabric-Cluster in Azure einrichten."
   services="service-fabric"
   documentationCenter=".net"
   authors="chackdan"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="vturecek"/>

# Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager

> [AZURE.SELECTOR]
- [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
- [Azure-Portal](service-fabric-cluster-creation-via-portal.md)

Diese Schrittanleitung führt Sie durch die Einrichtung eines sicheren Azure Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager. In diesem Leitfaden werden die folgenden Schritte behandelt:

 - Einrichten von Key Vault zum Verwalten von Schlüsseln für die Sicherheit von Clustern und Anwendungen.
 - Erstellen eines gesicherten Clusters in Azure mit Azure Resource Manager.
 - Authentifizieren von Benutzern mit Azure Active Directory (AAD) für die Clusterverwaltung.

Ein sicherer Cluster ist ein Cluster, der nicht autorisierten Zugriff auf Verwaltungsvorgänge verhindert, wie z.B. das Bereitstellen, Aktualisieren und Löschen von Anwendungen, Diensten und den darin enthaltenen Daten. Ein unsicherer Cluster ist ein Cluster, mit dem jeder Benutzer jederzeit eine Verbindung herstellen kann, um Verwaltungsvorgänge auszuführen. Die Erstellung eines unsicheren Clusters ist zwar möglich, allerdings **wird dringend empfohlen, einen sicheren Cluster zu erstellen**. Ein unsicherer Cluster **kann nicht zu einem späteren Zeitpunkt gesichert werden**. Stattdessen muss ein neuer Cluster erstellt werden.

## Anmelden an Azure
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

## Einrichten von Key Vault

Dieser Teil des Leitfadens führt Sie durch die Erstellung eines Schlüsseltresors für einen Service Fabric-Cluster in Azure und für Service Fabric-Anwendungen. Eine vollständige Anleitung zu Key Vault finden Sie unter [Erste Schritte mit dem Azure-Schlüsseltresor][key-vault-get-started].

Service Fabric verwendet X.509-Zertifikate, um einen Cluster zu sichern und Sicherheitsfunktionen für Anwendungen bereitzustellen. Zertifikate für Service Fabric-Cluster in Azure werden in Azure Key Vault verwaltet. Wenn ein Cluster in Azure bereitgestellt wird, ruft der für die Erstellung von Service Fabric-Clustern zuständige Azure-Ressourcenanbieter Zertifikate aus dem Schlüsseltresor ab und installiert sie auf den virtuellen Clustercomputern.

Das folgende Diagramm veranschaulicht die Beziehung zwischen dem Schlüsseltresor, einem Service Fabric-Cluster und dem Azure-Ressourcenanbieter, der beim Erstellen eines Clusters die in Key Vault gespeicherten Zertifikate verwendet:

![Zertifikatinstallation][cluster-security-cert-installation]

### Erstellen einer Ressourcengruppe

Der erste Schritt besteht darin, eine neue Ressourcengruppe speziell für den Schlüsseltresor zu erstellen. Es empfiehlt sich, den Schlüsseltresor in eine eigene Ressourcengruppe einzufügen, sodass Sie Compute- und Speicherressourcengruppen – wie z.B. die Ressourcengruppe, in der sich Ihr Service Fabric-Cluster befindet – entfernen können, ohne dass Ihre Schlüssel und geheimen Schlüssel verloren gehen. Die Ressourcengruppe, die Ihren Schlüsseltresor enthält, muss sich in der gleichen Region befinden wie der Cluster, der den Tresor verwendet.

```powershell

	New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
	WARNING: The output object type of this cmdlet will be modified in a future release.
	
	ResourceGroupName : mycluster-keyvault
	Location          : westus
	ProvisioningState : Succeeded
	Tags              :
	ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

### Erstellen eines Schlüsseltresors 

Erstellen Sie einen Schlüsseltresor in der neuen Ressourcengruppe. Der Schlüsseltresor **muss für die Bereitstellung aktiviert sein**, damit der Service Fabric-Ressourcenanbieter Zertifikate daraus abrufen und diese auf Clusterknoten installieren kann:

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


## Hinzufügen von Zertifikaten zum Schlüsseltresor

Zertifikate werden in Service Fabric zur Authentifizierung und Verschlüsselung verwendet, um verschiedene Aspekte eines Clusters und der zugehörigen Anwendungen zu sichern. Weitere Informationen zur Verwendung von Zertifikaten in Service Fabric finden Sie unter [Szenarien für die Clustersicherheit in Service Fabric][service-fabric-cluster-security].

### Cluster- und Serverzertifikat (erforderlich) 

Dieses Zertifikat wird benötigt, um einen Cluster zu sichern und nicht autorisierte Zugriffe auf den Cluster zu verhindern. Es sorgt auf unterschiedliche Weise für Clustersicherheit:
 
 - **Clusterauthentifizierung**: Authentifiziert die Kommunikation zwischen Knoten für einen Clusterverbund. Nur Knoten, die ihre Identität mit diesem Zertifikat nachweisen können, dürfen dem Cluster beitreten.
 - **Serverauthentifizierung**: Authentifiziert die Verwaltungsendpunkte des Clusters bei einem Verwaltungsclient, sodass der Verwaltungsclient weiß, dass die Kommunikation tatsächlich aus dem Cluster stammt. Dieses Zertifikat stellt auch SSL für die HTTPS-Verwaltungs-API und für Service Fabric Explorer über HTTPS bereit.

Für diese Zwecke muss das Zertifikat die folgenden Anforderungen erfüllen:

 - Das Zertifikat muss einen privaten Schlüssel enthalten.
 - Das Zertifikat muss für den Schlüsselaustausch erstellt werden und in eine PFX-Datei (Persönlicher Informationsaustausch) exportiert werden können.
 - Der Name des Antragstellers für das Zertifikat muss der Domäne entsprechen, über die auf den Service Fabric-Cluster zugegriffen wird. Dies ist erforderlich, damit SSL für die HTTPS-Verwaltungsendpunkte des Clusters und für Service Fabric Explorer bereitgestellt werden kann. Für die Domäne `.cloudapp.azure.com` können Sie kein SSL-Zertifikat von einer Zertifizierungsstelle beziehen. Sie benötigen einen benutzerdefinierten Domänennamen für Ihren Cluster. Wenn Sie ein Zertifikat von einer Zertifizierungsstelle anfordern, muss der Name des Antragstellers für das Zertifikat dem benutzerdefinierten Domänennamen entsprechen, der für Ihren Cluster verwendet wird.

### Anwendungszertifikate (optional)

Zum Zweck der Anwendungssicherheit kann eine beliebige Anzahl zusätzlicher Zertifikate in einem Cluster installiert werden. Bevor Sie den Cluster erstellen, betrachten Sie die verschiedenen Szenarien zur Anwendungssicherheit, in denen ein Zertifikat auf den Knoten installiert werden muss, beispielsweise:

 - Verschlüsselung und Entschlüsselung von Anwendungskonfigurationswerten
 - Knotenübergreifende Verschlüsselung von Daten während der Replikation

### Formatieren von Zertifikaten für die Verwendung durch einen Azure-Ressourcenanbieter

Private Schlüsseldateien (.pfx) können direkt über Key Vault hinzugefügt und verwendet werden. Für den Azure-Ressourcenanbieter müssen Schlüssel jedoch in einem bestimmten JSON-Format gespeichert werden, das die PFX-Datei, eine Base64-codierte Zeichenfolge und das Kennwort für den privaten Schlüssel enthält. Um diese Anforderungen zu erfüllen, müssen die Schlüssel in einer JSON-Zeichenfolge platziert und dann als *geheime Schlüssel* im Schlüsseltresor gespeichert werden.

Um diesen Prozess zu vereinfachen, ist ein PowerShell-Modul [auf GitHub verfügbar][service-fabric-rp-helpers]. Gehen Sie zur Verwendung des Moduls wie folgt vor:

  1. Laden Sie den gesamten Inhalt des Repositorys in ein lokales Verzeichnis herunter.
  2. Importieren Sie das Modul in Ihr PowerShell-Fenster:

  ```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
  ```
     
Der Befehl `Invoke-AddCertToKeyVault` in diesem PowerShell-Modul formatiert einen privaten Zertifikatschlüssel automatisch in eine JSON-Zeichenfolge und lädt diese in Key Vault hoch. Verwenden Sie diesen Befehl, um das Clusterzertifikat und weitere zusätzliche Anwendungszertifikate zum Schlüsseltresor hinzuzufügen. Wiederholen Sie diesen Schritt einfach für alle weiteren Zertifikate, die Sie in Ihrem Cluster installieren möchten.

```powershell
 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"
	
	Switching context to SubscriptionId <guid>
	Ensuring ResourceGroup mycluster-keyvault in West US
	WARNING: The output object type of this cmdlet will be modified in a future release.
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


Dies sind alle Voraussetzungen des Schlüsseltresors für die Konfiguration einer Resource Manager-Vorlage für Service Fabric-Cluster, die Zertifikate für die Knotenauthentifizierung, die Sicherheit und Authentifizierung von Verwaltungsendpunkten sowie weitere Sicherheitsfeatures für Anwendungen installiert, die X.509-Zertifikate verwenden. An diesem Punkt sollten Sie über folgendes Setup in Azure verfügen:

 - Schlüsseltresor-Ressourcengruppe
   - Schlüsseltresor
     - Authentifizierungszertifikat für den Clusterserver
     - Anwendungszertifikate

## Einrichten von Azure Active Directory für die Clientauthentifizierung

AAD ermöglicht Organisationen (als Mandanten bezeichnet) die Verwaltung des Benutzerzugriffs auf Anwendungen, die in Anwendungen mit einer webbasierten Anmeldebenutzeroberfläche und Anwendungen mit einer nativen Clientbenutzeroberfläche unterteilt sind. In diesem Dokument wird davon ausgegangen, dass Sie bereits einen Mandanten erstellt haben. Falls nicht, lesen Sie zuerst den Artikel [Einrichten eines Azure Active Directory-Mandanten][active-directory-howto-tenant].

Service Fabric-Cluster bieten unterschiedliche Einstiegspunkte für ihre Verwaltungsfunktionen. Hierzu zählen etwa der webbasierte [Service Fabric Explorer][service-fabric-visualizing-your-cluster] und [Visual Studio][service-fabric-manage-application-in-visual-studio]. Daher erstellen Sie zwei AAD-Anwendungen, um den Zugriff auf den Cluster zu steuern: eine Webanwendung und eine native Anwendung.

Wir haben einen Satz mit Windows PowerShell-Skripts erstellt, um einige Schritte der Konfiguration von AAD mit einem Service Fabric-Cluster zu vereinfachen.

>[AZURE.NOTE] Diese Schritte müssen *vor* der Clustererstellung ausgeführt werden. Falls für die Skripts Clusternamen und Endpunkte verwendet werden, sollte es sich dabei also um geplante Werte handeln, nicht um bereits erstellte Werte.

1. [Laden Sie die Skripts auf Ihren Computer herunter][sf-aad-ps-script-download].

2. Klicken Sie mit der rechten Maustaste auf die ZIP-Datei, wählen Sie **Eigenschaften** aus, aktivieren Sie das Kontrollkästchen **Zulassen**, und übernehmen Sie die Änderung.

3. Extrahieren Sie die ZIP-Datei.

4. Führen Sie `SetupApplications.ps1` aus, und geben Sie TenantId, ClusterName und WebApplicationReplyUrl als Parameter an. Beispiel:

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    Die **TenantId** können Sie anhand der URL für den Mandanten im klassischen Azure-Portal ermitteln. Die GUID, die in diese URL eingebettet ist, ist die TenantId. Beispiel:

    https://<i></i>manage.windowsazure.com/microsoft.onmicrosoft.com#Workspaces/ActiveDirectoryExtension/Directory/**690ec069-8200-4068-9d01-5aaf188e557a**/users

    **ClusterName** wird verwendet, um die vom Skript erstellten AAD-Anwendungen mit einem Präfix zu versehen. Dies muss keine exakte Übereinstimmung mit dem tatsächlichen Clusternamen sein. Er dient lediglich als Hilfe, um Ihnen die Zuordnung von AAD-Artefakten zu dem Service Fabric-Cluster zu erleichtern, für den sie verwendet werden.

    **WebApplicationReplyUrl** ist der Standardendpunkt, an den Ihre Benutzer von AAD nach Abschluss des Anmeldevorgangs zurückgeleitet werden. Sie sollten diesen Wert auf den Service Fabric Explorer-Endpunkt für Ihren Cluster festlegen, also standardmäßig:

    https://&lt;cluster_domain&gt;:19080/Explorer

    Sie werden aufgefordert, sich bei einem Konto anzumelden, das über Administratorrechte für den AAD-Mandanten verfügt. Nach der Anmeldung erstellt das Skript die Webanwendung und native Anwendung, um Ihren Service Fabric-Cluster darzustellen. Wenn Sie sich die Anwendungen des Mandanten im [klassischen Azure-Portal][azure-classic-portal] ansehen, sollten zwei neue Einträge angezeigt werden:

    - *ClusterName*\_Cluster
    - *ClusterName*\_Client

    Das Skript gibt den für die Azure Resource Manager-Vorlage erforderlichen JSON-Code aus, wenn Sie den Cluster im nächsten Abschnitt erstellen. Lassen Sie das PowerShell-Fenster also geöffnet.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## Erstellen einer Resource Manager-Vorlage für Service Fabric-Cluster

In diesem Abschnitt wird die Ausgabe der vorherigen PowerShell-Befehle in einer Resource Manager-Vorlage für Service Fabric-Cluster verwendet.

Resource Manager-Beispielvorlagen stehen im [Azure-Schnellstart-Vorlagenkatalog auf GitHub][azure-quickstart-templates] zur Verfügung. Diese Vorlagen können als Ausgangspunkt für Ihre Clustervorlage verwendet werden.

### Erstellen der Resource Manager-Vorlage

In diesem Leitfaden werden die Beispielvorlage [service-fabric-secure-cluster-5-node-1-nodetype-wad][service-fabric-secure-cluster-5-node-1-nodetype-wad] und die zugehörigen Vorlagenparameter verwendet. Laden Sie `azuredeploy.json` und `azuredeploy.parameters.json` auf Ihren Computer herunter, und öffnen Sie beide Dateien in einem Texteditor.

### Hinzufügen von Zertifikaten

Sie fügen Zertifikate zu einer Resource Manager-Vorlage für einen Cluster hinzu, indem Sie auf den Schlüsseltresor verweisen, der die Zertifikatschlüssel enthält. Es empfiehlt sich, diese Schlüsseltresorwerte in einer Parameterdatei für die Resource Manager-Vorlage zu speichern, damit die Resource Manager-Vorlagendatei wiederverwendet werden kann und keine Werte enthält, die spezifisch für eine bestimmte Bereitstellung gelten.

#### Hinzufügen aller Zertifikate zu VMSS osProfile

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

#### Konfigurieren des Service Fabric-Clusterzertifikats

Das Zertifikat für die Clusterauthentifizierung muss auch in der Service Fabric-Clusterressource (Microsoft.ServiceFabric/clusters) sowie in der Service Fabric-Erweiterung für VMSS in der VMSS-Ressource konfiguriert werden. Dies ermöglicht dem Service Fabric-Ressourcenanbieter, das Zertifikat für die Clusterauthentifizierung und die Serverauthentifizierung für Verwaltungsendpunkte zu konfigurieren.

##### VMSS-Ressource:

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

### Einfügen der AAD-Konfiguration

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

### Konfigurieren von Resource Manager-Vorlagenparametern

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

 - Schlüsseltresor-Ressourcengruppe
    - Schlüsseltresor
    - Authentifizierungszertifikat für den Clusterserver
    - Datenverschlüsselungszertifikat
 - Azure Active Directory-Mandant
    - AAD-Anwendung für webbasierte Verwaltung und Service Fabric Explorer
    - AAD-Anwendung für native Clientverwaltung
    - Benutzer mit zugewiesenen Rollen
 - Resource Manager-Vorlage für Service Fabric-Cluster
    - In Key Vault konfigurierte Zertifikate
    - Konfiguriertes Azure Active Directory

Das folgende Diagramm veranschaulicht die Platzierung der Key Vault- und AAD-Konfiguration in der Resource Manager-Vorlage.

![Abhängigkeiten in Resource Manager][cluster-security-arm-dependency-map]

## Cluster erstellen

Jetzt ist alles für vorbereitet, und Sie können den Cluster mithilfe der [ARM-Bereitstellung][resource-group-template-deploy] erstellen.

#### Testen

Verwenden Sie den folgenden PowerShell-Befehl, um Ihre Resource Manager-Vorlage mit einer Parameterdatei zu testen:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### Bereitstellen

Wenn der Test der Resource Manager-Vorlage erfolgreich verläuft, verwenden Sie den folgenden PowerShell-Befehl, um Ihre Resource Manager-Vorlage mit einer Parameterdatei bereitzustellen:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

## Zuweisen von Benutzern zu Rollen

Nachdem Sie die Anwendungen zum Darstellen Ihres Clusters erstellt haben, müssen Sie Ihre Benutzer den Rollen zuweisen, die von Service Fabric unterstützt werden: „read-only“ (schreibgeschützt) und „admin“ (Administrator). Hierfür können Sie das [klassische Azure-Portal][azure-classic-portal] verwenden.

1. Navigieren Sie zu Ihrem Mandanten, und wählen Sie „Anwendungen“.
2. Wählen Sie die Webanwendung aus, die hier über einen Namen wie `myTestCluster_Cluster` verfügt.
3. Klicken Sie auf die Registerkarte „Benutzer“.
4. Wählen Sie einen zuzuweisenden Benutzer aus, und klicken Sie am unteren Bildschirmrand auf die Schaltfläche **Zuweisen**.

    ![Schaltfläche „Benutzer zu Rollen zuweisen“][assign-users-to-roles-button]

5. Wählen Sie die Rolle aus, die dem Benutzer zugewiesen werden soll.

    ![Zuweisen von Benutzern zu Rollen][assign-users-to-roles-dialog]

>[AZURE.NOTE] Weitere Informationen zu Rollen in Service Fabric finden Sie unter [Rollenbasierte Zugriffssteuerung für Service Fabric-Clients](service-fabric-cluster-security-roles.md).


## Nächste Schritte

Sie verfügen jetzt über einen sicheren Cluster, für den Azure Active Directory die Authentifizierung für die Verwaltung bereitstellt. Als Nächstes [stellen Sie eine Verbindung mit dem Cluster her](service-fabric-connect-to-secure-cluster.md), und erfahren, wie Sie [geheime Anwendungsschlüssel verwalten](service-fabric-application-secret-management.md).

<!-- Links -->
[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]: ../key-vault/key-vault-get-started.md
[aad-graph-api-docs]: https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]: http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype-wad]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype-wad/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png

<!---HONumber=AcomDC_0921_2016-->