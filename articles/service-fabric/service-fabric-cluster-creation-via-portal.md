
<properties
   pageTitle="Erstellen eines sicheren Service Fabric-Clusters über das Azure-Portal | Microsoft Azure"
   description="Dieser Artikel beschreibt, wie Sie über das Azure-Portal und mithilfe von Azure Key Vault einen sicheren Service Fabric-Cluster in Azure erstellen."
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
   ms.date="09/21/2016"
   ms.author="vturecek"/>

# Erstellen eines Service Fabric-Clusters in Azure über das Azure-Portal

> [AZURE.SELECTOR]
- [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
- [Azure-Portal](service-fabric-cluster-creation-via-portal.md)

Diese Schrittanleitung führt Sie durch die Einrichtung eines sicheren Service Fabric-Clusters in Azure über das Azure-Portal. In diesem Leitfaden werden die folgenden Schritte behandelt:

 - Einrichten von Key Vault zum Verwalten von Schlüsseln für die Sicherheit von Clustern.
 - Erstellen eines gesicherten Clusters in Azure über das Azure-Portal.
 - Authentifizieren von Administratoren mithilfe von Zertifikaten.

>[AZURE.NOTE] Um erweiterte Sicherheitsoptionen zu nutzen, wie z.B. Benutzerauthentifizierung mit Azure Active Directory und Zertifikate für die Anwendungssicherheit, [erstellen Sie Ihren Cluster mithilfe von Azure Resource Manager][create-cluster-arm].

Ein sicherer Cluster ist ein Cluster, der nicht autorisierten Zugriff auf Verwaltungsvorgänge verhindert, wie z.B. das Bereitstellen, Aktualisieren und Löschen von Anwendungen, Diensten und den darin enthaltenen Daten. Ein unsicherer Cluster ist ein Cluster, mit dem jeder Benutzer jederzeit eine Verbindung herstellen kann, um Verwaltungsvorgänge auszuführen. Die Erstellung eines unsicheren Clusters ist zwar möglich, allerdings **wird dringend empfohlen, einen sicheren Cluster zu erstellen**. Ein unsicherer Cluster **kann nicht zu einem späteren Zeitpunkt gesichert werden**. Stattdessen muss ein neuer Cluster erstellt werden.

Die gleichen Konzepte kommen auch beim Erstellen sicherer Linux- oder Windows-Cluster zur Anwendung. Weitere Informationen und Hilfsskripts zur Erstellung sicherer Linux-Cluster finden Sie unter [Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager](service-fabric-cluster-creation-via-arm.md#secure-linux-cluster). Die von dem Hilfsskript ermittelten Parameter können direkt im Portal angegeben werden, wie im Abschnitt [Erstellen eines Clusters im Azure-Portal](#create-cluster-portal) beschrieben.

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

Service Fabric verwendet X.509-Zertifikate, um einen Cluster zu sichern. Zertifikate für Service Fabric-Cluster in Azure werden in Azure Key Vault verwaltet. Wenn ein Cluster in Azure bereitgestellt wird, ruft der für die Erstellung von Service Fabric-Clustern zuständige Azure-Ressourcenanbieter Zertifikate aus dem Schlüsseltresor ab und installiert sie auf den virtuellen Clustercomputern.

Das folgende Diagramm veranschaulicht die Beziehung zwischen dem Schlüsseltresor, einem Service Fabric-Cluster und dem Azure-Ressourcenanbieter, der beim Erstellen eines Clusters die in Key Vault gespeicherten Zertifikate verwendet:

![Zertifikatinstallation][cluster-security-cert-installation]

### Erstellen einer Ressourcengruppe

Der erste Schritt besteht darin, eine neue Ressourcengruppe speziell für den Schlüsseltresor zu erstellen. Es empfiehlt sich, den Schlüsseltresor in eine eigene Ressourcengruppe einzufügen, sodass Sie Compute- und Speicherressourcengruppen – wie z.B. die Ressourcengruppe, in der sich Ihr Service Fabric-Cluster befindet – entfernen können, ohne dass Ihre Schlüssel und geheimen Schlüssel verloren gehen. Die Ressourcengruppe, die Ihren Schlüsseltresor enthält, muss sich in der gleichen Region befinden wie der Cluster, der den Tresor verwendet.

```powershell

	PS C:\Users\vturecek> New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
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

	PS C:\Users\vturecek> New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment
	
	
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
 - Der Name des Antragstellers für das Zertifikat muss der Domäne entsprechen, über die auf den Service Fabric-Cluster zugegriffen wird. Dies ist erforderlich, damit SSL für die HTTPS-Verwaltungsendpunkte des Clusters und für Service Fabric Explorer bereitgestellt werden kann. Für die Domäne `.cloudapp.azure.com` können Sie kein SSL-Zertifikat von einer Zertifizierungsstelle beziehen. Erwerben Sie einen benutzerdefinierten Domänennamen für Ihren Cluster. Wenn Sie ein Zertifikat von einer Zertifizierungsstelle anfordern, muss der Name des Antragstellers für das Zertifikat dem benutzerdefinierten Domänennamen entsprechen, der für Ihren Cluster verwendet wird.

### Clientauthentifizierungszertifikate

Zusätzliche Clientzertifikate authentifizieren Administratoren für Clusterverwaltungsaufgaben. Service Fabric verfügt über zwei Zugriffsebenen: **Administrator** und **Schreibgeschützter Benutzer**. Es sollte mindestens ein einzelnes Zertifikat für den Administratorzugriff verwendet werden. Um weiteren Zugriff auf Benutzerebene zu ermöglichen, muss ein separates Zertifikat bereitgestellt werden. Weitere Informationen zu Zugriffsrollen finden Sie unter [Rollenbasierte Zugriffssteuerung für Service Fabric-Clients][service-fabric-cluster-security-roles].

Zur Verwendung von Service Fabric müssen keine Clientauthentifizierungszertifikate in Key Vault hochgeladen werden. Diese Zertifikate müssen nur für Benutzer bereitgestellt werden, die zur Clusterverwaltung autorisiert sind.

>[AZURE.NOTE] Zur Authentifizierung von Clients für Clusterverwaltungsvorgänge wird Azure Active Directory empfohlen. Um Azure Active Directory verwenden zu können, müssen Sie [den Cluster mithilfe von Azure Resource Manager erstellen][create-cluster-arm].

### Anwendungszertifikate (optional)

Zum Zweck der Anwendungssicherheit kann eine beliebige Anzahl zusätzlicher Zertifikate in einem Cluster installiert werden. Bevor Sie den Cluster erstellen, betrachten Sie die verschiedenen Szenarien zur Anwendungssicherheit, in denen ein Zertifikat auf den Knoten installiert werden muss, beispielsweise:

 - Verschlüsselung und Entschlüsselung von Anwendungskonfigurationswerten
 - Knotenübergreifende Verschlüsselung von Daten während der Replikation

Anwendungszertifikate können nicht konfiguriert werden, wenn Sie einen Cluster über das Azure-Portal erstellen. Um Anwendungszertifikate während der Einrichtung eines Clusters zu konfigurieren, müssen Sie [den Cluster mithilfe von Azure Resource Manager erstellen][create-cluster-arm]. Sie können dem Cluster nach der Erstellung auch Anwendungszertifikate hinzufügen.

### Formatieren von Zertifikaten für die Verwendung durch einen Azure-Ressourcenanbieter

Private Schlüsseldateien (.pfx) können direkt über Key Vault hinzugefügt und verwendet werden. Für den Azure-Ressourcenanbieter müssen Schlüssel jedoch in einem bestimmten JSON-Format gespeichert werden, das die PFX-Datei, eine Base64-codierte Zeichenfolge und das Kennwort für den privaten Schlüssel enthält. Um diese Anforderungen zu erfüllen, müssen die Schlüssel in einer JSON-Zeichenfolge platziert und dann als *geheime Schlüssel* im Schlüsseltresor gespeichert werden.

Um diesen Prozess zu vereinfachen, ist ein PowerShell-Modul [auf GitHub verfügbar][service-fabric-rp-helpers]. Gehen Sie zur Verwendung des Moduls wie folgt vor:

 1. Laden Sie den gesamten Inhalt des Repositorys in ein lokales Verzeichnis herunter.
 2. Importieren Sie das Modul in Ihr PowerShell-Fenster:

```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
```
     
Der Befehl `Invoke-AddCertToKeyVault` in diesem PowerShell-Modul formatiert einen privaten Zertifikatschlüssel automatisch in eine JSON-Zeichenfolge und lädt diese in Key Vault hoch. Verwenden Sie diesen Befehl, um das Clusterzertifikat und weitere zusätzliche Anwendungszertifikate zum Schlüsseltresor hinzuzufügen. Wiederholen Sie diesen Schritt für alle weiteren Zertifikate, die Sie in Ihrem Cluster installieren möchten.

```powershell
PS C:\Users\vturecek> Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"
	
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

</a "create-cluster-portal" ></a>
## Erstellen eines Clusters im Azure-Portal

### Suchen nach der Service Fabric-Clusterressource

![Suche nach der Service Fabric-Clustervorlage im Azure-Portal.][SearchforServiceFabricClusterTemplate]

 1. Melden Sie sich auf dem [Azure-Portal][azure-portal] an.

 2. Klicken Sie auf **Neu**, um eine neue Ressourcenvorlage hinzuzufügen. Suchen Sie Ihre Service Fabric Cluster-Vorlage im **Marketplace** unter **Alles**.

 3. Wählen Sie **Service Fabric-Cluster** aus der Liste aus.

 4. Wechseln Sie zum Blatt **Service Fabric-Cluster**, und klicken Sie auf **Erstellen**.

 5. Das Blatt **Service Fabric-Cluster erstellen** umfasst vier Schritte:

#### 1\. Grundlagen

![Screenshot der Erstellung einer neuen Ressourcengruppe.][CreateRG]

Auf dem Blatt mit den Grundeinstellungen müssen die grundlegenden Details für Ihren Cluster angegeben werden.

 1. Geben Sie den Namen Ihres Clusters ein.

 2. Geben Sie einen **Benutzernamen** und ein **Kennwort** für Remotedesktop für die virtuellen Computer.

 3. Stellen Sie sicher, dass Sie das gewünschte **Abonnement** auswählen, in dem Ihr Cluster bereitgestellt werden soll. Dies ist besonders wichtig, wenn Sie über mehrere Abonnements verfügen.

 4. Erstellen Sie eine neue **Ressourcengruppe**. Verwenden Sie für die Ressourcengruppe den gleichen Namen wie für den Cluster, um sie später einfacher wiederzufinden. Das ist besonders dann hilfreich, wenn Sie Änderungen an Ihrer Bereitstellung vornehmen oder Ihren Cluster löschen möchten.

    >[AZURE.NOTE] Auch wenn Sie eine vorhandene Ressourcengruppe verwenden können, empfiehlt es sich, eine neue Ressourcengruppe zu erstellen. So können Sie leichter die Cluster löschen, die Sie nicht mehr benötigen.

 5. Wählen Sie die **Region** aus, in der Sie den Cluster erstellen möchten. Sie müssen die gleiche Region verwenden, in der sich Ihr Schlüsseltresor befindet.

#### 2\. Clusterkonfiguration

![Erstellen eines Knotentyps][CreateNodeType]

Konfigurieren Sie die Clusterknoten. Knotentypen definieren die Größe, die Anzahl und die Eigenschaften der virtuellen Computer. Der Cluster kann über mehrere Knotentypen verfügen. Der primäre Knotentyp (der erste, den Sie im Portal definieren), muss jedoch über mindestens fünf virtuelle Computer verfügen, da dies der Knotentyp ist, in dem Service Fabric-Systemdienste platziert werden. Konfigurieren Sie keine **Platzierungseigenschaften**. Die Standardplatzierungseigenschaft „NodeTypeName“ wird automatisch hinzugefügt.

   >[AZURE.NOTE] Ein gängiges Szenario für mehrere Knotentypen ist eine Anwendung, die einen Front-End-Dienst und einen Back-End-Dienst enthält. Der Front-End-Dienst soll auf kleineren virtuellen Computern (VM-Größen wie z.B. D2) ausgeführt werden, auf denen Ports für das Internet geöffnet sind. Der Back-End-Dienst soll auf größeren virtuellen Computern (VM-Größen wie z.B. D4, D6, D15 usw.) platziert werden, auf denen keine Ports für das Internet geöffnet sind.

 1. Wählen Sie einen Namen für Ihren Knotentyp aus (ein bis zwölf Zeichen, nur Buchstaben und Zahlen).

 2. Die **Mindestgröße** von VMs für den primären Knotentyp hängt von der **Dauerhaftigkeitsstufe** ab, die Sie für den Cluster wählen. Der Standardwert für die Dauerhaftigkeitsstufe ist „Bronze“. Weitere Informationen zur Dauerhaftigkeit finden Sie unter [Überlegungen zur Kapazitätsplanung für Service Fabric-Cluster][service-fabric-cluster-capacity].

 3. Wählen Sie die VM-Größe und den gewünschten Tarif aus. VMs der D-Serie verfügen über SSDs (Solid-State Drives) und werden für zustandsbehaftete Anwendungen sehr empfohlen. Verwenden Sie keine SKU für virtuelle Computer, die über Teilkerne oder über weniger als 7 GB an verfügbarem Speicherplatz verfügt.

 4. Die **Mindestanzahl** von VMs für den primären Knotentyp hängt von der **Zuverlässigkeitsstufe** ab, die Sie für den Cluster wählen. Der Standardwert für die Zuverlässigkeitsstufe ist „Silber“. Weitere Informationen zur Zuverlässigkeit finden Sie unter [Überlegungen zur Kapazitätsplanung für Service Fabric-Cluster][service-fabric-cluster-capacity].

 5. Wählen Sie die Anzahl von VMs für den Knotentyp aus. Sie können die Anzahl von VMs auf einem Knotentyp später zentral hoch- oder herunterskalieren, beim primären Knotentyp wird die Mindestanzahl jedoch durch die gewählte Zuverlässigkeitsstufe bestimmt. Andere Knotentypen können über mindestens einen virtuellen Computer verfügen.

 6. Konfigurieren Sie benutzerdefinierte Endpunkte. In diesem Feld können Sie eine durch Trennzeichen getrennte Liste der Ports eingeben, die Sie über den Azure Load Balancer verfügbar machen möchten, damit Ihre Anwendungen auf das öffentliche Internet zugreifen können. Wenn Sie z.B. die Bereitstellung einer Webanwendung in Ihrem Cluster planen, geben Sie hier „80“ ein, um Datenverkehr in Ihren Cluster über Port 80 zuzulassen. Weitere Informationen zu Endpunkten finden Sie unter [Herstellung einer Verbindung mit Diensten in Service Fabric und die Kommunikation mit diesen Diensten][service-fabric-connect-and-communicate-with-services].

 7. Konfigurieren der **Diagnose** für Cluster. Die Diagnose ist standardmäßig in Ihrem Cluster aktiviert, um die Behebung von Problemen in Ihrem Cluster zu vereinfachen. Zum Deaktivieren der Diagnose ändern Sie den **Status** in **Aus**. Das Ausschalten der Diagnose wird **nicht** empfohlen.

 9. Wählen Sie den gewünschten Fabric-Upgrademodus für Ihren Cluster aus. Wählen Sie **Automatisch** aus, wenn für Ihren Cluster automatisch ein Upgrade auf die neueste verfügbare Version durchgeführt werden soll. Legen Sie den Modus auf **Manuell** fest, wenn Sie selbst eine unterstützte Version auswählen möchten.

>[AZURE.NOTE] Es werden nur Cluster mit einer unterstützten Service Fabric-Version unterstützt. Bei Verwendung des manuellen Modus müssen Sie Ihren Cluster eigenverantwortlich auf eine unterstützte Version upgraden. Weitere Informationen zum Fabric-Upgrademodus finden Sie unter [Upgrade von Service Fabric-Clustern][service-fabric-cluster-upgrade].


#### 3\. Sicherheit

![Screenshot der Sicherheitskonfigurationen im Azure-Portal.][SecurityConfigs]

Der letzte Schritt besteht darin, Zertifikatinformationen bereitzustellen, um den Cluster mithilfe der zuvor erstellten Schlüsseltresor- und Zertifikatinformationen zu sichern.

 
- Füllen Sie die Felder für das primäre Zertifikat mit der Ausgabe des PowerShell-Befehls `Invoke-AddCertToKeyVault` auf, den Sie zum Hochladen des **Clusterzertifikats** in den Schlüsseltresor verwendet haben.

```powershell
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47
```

- Aktivieren Sie das Kontrollkästchen **Erweiterte Einstellungen konfigurieren**, um Clientzertifikate für den **Verwaltungsclient** und den **schreibgeschützten Client** einzugeben. Geben Sie in diesen Feldern ggf. den Fingerabdruck des Verwaltungsclientzertifikats und den Fingerabdruck des schreibgeschützten Benutzerclientzertifikats ein. Wenn Administratoren versuchen, eine Verbindung mit dem Cluster herzustellen, wird ihnen nur dann Zugriff gewährt, wenn sie über ein Zertifikat mit einem Fingerabdruck verfügen, der mit den hier angegebenen Fingerabdruckwerten übereinstimmt.


#### 4\. Zusammenfassung

![Screenshot des Startmenüs mit der Anzeige „Service Fabric-Cluster wird bereitgestellt“.][Notifications]

Klicken Sie zum Abschließen der Clustererstellung auf **Zusammenfassung**, um die angegebene Konfiguration anzuzeigen, oder laden Sie die Azure Resource Manager-Vorlage herunter, die zur Bereitstellung des Clusters verwendet werden soll. Nach Angabe der erforderlichen Einstellungen wird die Schaltfläche **OK** grün dargestellt. Klicken Sie auf die Schaltfläche, um die Clustererstellung zu starten.

Sie können den Verlauf der Erstellung in den Benachrichtigungen finden. (Klicken Sie auf das Glockensymbol in der Nähe der Statusleiste am oberen rechten Bildschirmrand.) Wenn Sie beim Erstellen des Clusters auf **An Startmenü anheften** geklickt haben, wird im **Startmenü** der Hinweis **Service Fabric-Cluster wird bereitgestellt** angezeigt.

### Anzeigen des Clusterstatus

![Screenshot der Clusterdetails im Dashboard.][ClusterDashboard]

Sobald der Cluster erstellt wurde, können Sie Ihren Cluster im Portal überprüfen.

 1. Wechseln Sie zu **Durchsuchen**, und klicken Sie auf **Service Fabric-Cluster**.

 2. Suchen Sie Ihren Cluster, und klicken Sie darauf.

 3. Jetzt sehen Sie die Details Ihres Clusters im Dashboard, einschließlich des öffentlichen Endpunkts des Clusters und eines Links zu Service Fabric Explorer.

Der Abschnitt **Knotenmonitor** auf dem Dashboardblatt des Clusters gibt die Anzahl von virtuellen Computern an, die fehlerfrei bzw. fehlerhaft sind. Weitere Informationen zur Clusterintegrität finden Sie unter [Einführung in die Service Fabric-Integritätsüberwachung][service-fabric-health-introduction].

>[AZURE.NOTE] Um die Verfügbarkeit zu gewährleisten und den Zustand beizubehalten, muss eine bestimmte Anzahl von Knoten in einem Service Fabric-Cluster stets in Betrieb sein. Dies wird auch als „Aufrechterhalten eines Quorums“ bezeichnet. Aus Sicherheitsgründen empfiehlt es sich in der Regel nicht, alle Computer innerhalb des Clusters herunterzufahren, sofern Sie zuvor keine [vollständige Sicherung des Zustands][service-fabric-reliable-services-backup-restore] durchgeführt haben.

## Herstellen einer Remoteverbindung mit einer Instanz der VM-Skalierungsgruppe oder einem Clusterknoten

Für jeden Knotentyp, den Sie in Ihrem Cluster angeben, wird jeweils eine VM-Skalierungsgruppe eingerichtet. Weitere Informationen finden Sie unter [Herstellen einer Remoteverbindung mit einer VM-Skalierungsgruppeninstanz oder einem Clusterknoten][remote-connect-to-a-vm-scale-set].

## Nächste Schritte

Sie verfügen jetzt über einen sicheren Cluster, der Zertifikate zur Verwaltungsauthentifizierung verwendet. Als Nächstes [stellen Sie eine Verbindung mit dem Cluster her](service-fabric-connect-to-secure-cluster.md), und erfahren, wie Sie [geheime Anwendungsschlüssel verwalten](service-fabric-application-secret-management.md).


<!-- Links -->
[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[azure-portal]: https://portal.azure.com/
[key-vault-get-started]: ../key-vault/key-vault-get-started.md
[create-cluster-arm]: https://manage.windowsazure.com
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[service-fabric-cluster-security-roles]: service-fabric-cluster-security-roles.md
[service-fabric-cluster-capacity]: service-fabric-cluster-capacity.md
[service-fabric-connect-and-communicate-with-services]: service-fabric-connect-and-communicate-with-services.md
[service-fabric-health-introduction]: service-fabric-health-introduction.md
[service-fabric-reliable-services-backup-restore]: service-fabric-reliable-services-backup-restore.md
[remote-connect-to-a-vm-scale-set]: service-fabric-cluster-nodetypes.md#remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node
[service-fabric-cluster-upgrade]: service-fabric-cluster-upgrade.md

<!--Image references-->
[SearchforServiceFabricClusterTemplate]: ./media/service-fabric-cluster-creation-via-portal/SearchforServiceFabricClusterTemplate.png
[CreateRG]: ./media/service-fabric-cluster-creation-via-portal/CreateRG.png
[CreateNodeType]: ./media/service-fabric-cluster-creation-via-portal/NodeType.png
[SecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/SecurityConfigs.png
[Notifications]: ./media/service-fabric-cluster-creation-via-portal/notifications.png
[ClusterDashboard]: ./media/service-fabric-cluster-creation-via-portal/ClusterDashboard.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png

<!---HONumber=AcomDC_0928_2016-->