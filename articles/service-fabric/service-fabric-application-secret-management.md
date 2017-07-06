---
title: Verwalten von Geheimnissen in Service Fabric-Anwendungen | Microsoft-Dokumentation
description: "In diesem Artikel wird beschrieben, wie geheime Werte in einer Service Fabric-Anwendung geschützt werden."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 94a67e45-7094-4fbd-9c88-51f4fc3c523a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.translationtype: Human Translation
ms.sourcegitcommit: 0af5a4e2139a202c7f62f48c7a7e8552457ae76d
ms.openlocfilehash: cc6ef8f3ef5371ac3092686afddc9198516916fd
ms.contentlocale: de-de
ms.lasthandoff: 12/09/2016


---
# <a name="managing-secrets-in-service-fabric-applications"></a>Verwalten von geheimen Daten in Service Fabric-Anwendungen
In diesem Leitfaden werden die Schritte zum Verwalten von Geheimnissen in einer Service Fabric-Anwendung beschrieben. Geheimnisse beinhalten jegliche Art von vertraulichen Informationen (z.B. Speicherverbindungszeichenfolgen, Kennwörter oder andere Werte, die nicht als Nur-Text verarbeitet werden sollen).

In diesem Leitfaden wird Azure Key Vault für die Verwaltung von Schlüsseln und Geheimnissen verwendet. Die *Verwendung* von Geheimnissen in einer Anwendung ist jedoch cloudplattformunabhängig, sodass Anwendungen auf einem Cluster bereitgestellt werden können, der an einem beliebigen Standort gehostet wird. 

## <a name="overview"></a>Übersicht
Es wird empfohlen, Dienstkonfigurationseinstellungen über [Dienstkonfigurationspakete][config-package] zu verwalten. Konfigurationspakete verfügen über eine Versionsangabe und können über parallele Upgrades aktualisiert werden. Außerdem kann die Integrität überprüft und ein automatischer Rollback durchgeführt werden. Dies wird der globalen Konfiguration vorgezogen, da die Wahrscheinlichkeit eines globalen Dienstausfalls verringert wird. Verschlüsselte Geheimnisse stellen keine Ausnahme dar. Service Fabric verfügt über integrierte Features zum Verschlüsseln und Entschlüsseln von Werten in der Konfigurationspaketdatei „Settings.xml“ mithilfe der Zertifikatverschlüsselung.

Das Diagramm unten zeigt den grundlegenden Ablauf bei der Verwaltung von Geheimnissen in einer Service Fabric-Anwendung:

![Übersicht über die Verwaltung von Geheimnissen][overview]

Dieser Vorgang besteht im Wesentlichen aus vier Schritten:

1. Abrufen eines Datenverschlüsselungszertifikats
2. Installieren des Zertifikats in Ihrem Cluster
3. Verschlüsseln von Geheimnissen bei der Bereitstellung einer Anwendung mit dem Zertifikat und Einfügen dieser Geheimnisse in die Konfigurationsdatei „Settings.xml“ des Diensts
4. Lesen der verschlüsselten Werte aus der Datei „Settings.xml“, indem diese mit demselben Verschlüsselungszertifikat entschlüsselt werden 

[Azure Key Vault][key-vault-get-started] wird als sicherer Speicherort für Zertifikate sowie zum Installieren von Zertifikaten auf Service Fabric-Clustern in Azure verwendet. Wenn die Bereitstellung nicht in Azure erfolgt, muss Key Vault nicht zum Verwalten von Geheimnissen in Service Fabric-Anwendungen eingesetzt werden.

## <a name="data-encipherment-certificate"></a>Datenverschlüsselungszertifikat
Ein Datenverschlüsselungszertifikat wird ausschließlich für die Verschlüsselung und Entschlüsselung von Konfigurationswerten in der Datei „Settings.xml“ eines Diensts und nicht für die Authentifizierung oder das Signieren des Verschlüsselungstexts verwendet. Das Zertifikat muss die folgenden Anforderungen erfüllen:

* Das Zertifikat muss einen privaten Schlüssel enthalten.
* Das Zertifikat muss für den Schlüsselaustausch erstellt werden und in eine PFX-Datei (Persönlicher Informationsaustausch) exportiert werden können.
* Zu den wichtigsten Verwendungszwecken des Zertifikatschlüssels muss die Datenverschlüsselung (10) zählen. Der Zertifikatschlüssel darf nicht für die Server- oder Clientauthentifizierung verwendet werden. 
  
  Beim Erstellen eines selbstsignierten Zertifikats mithilfe von PowerShell muss z.B. das Flag `KeyUsage` auf `DataEncipherment` festgelegt werden:
  
  ```powershell
  New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
  ```

## <a name="install-the-certificate-in-your-cluster"></a>Installieren des Zertifikats in Ihrem Cluster
Dieses Zertifikat muss auf jedem Knoten innerhalb des Clusters installiert werden. Es wird zur Laufzeit zum Entschlüsseln von Werten verwendet, die in der Datei „Settings.xml“ eines Diensts gespeichert sind. Anweisungen zum Einrichten finden Sie unter [Erstellen eines Clusters mithilfe von Azure Resource Manager][service-fabric-cluster-creation-via-arm]. 

## <a name="encrypt-application-secrets"></a>Verschlüsseln von Geheimnissen in Anwendungen
Das Service Fabric-SDK verfügt über integrierte Funktionen zum Verschlüsseln und Entschlüsseln von Geheimnissen. Geheime Werte können bei der Erstellung verschlüsselt und programmgesteuert im Dienstcode entschlüsselt und gelesen werden. 

Der folgende PowerShell-Befehl wird zum Verschlüsseln eines geheimen Werts verwendet. Mit diesem Befehl wird nur der Wert verschlüsselt. Der Verschlüsselungstext wird **nicht** verschlüsselt. Zum Erstellen des Chiffretexts für geheime Werte muss das Verschlüsselungszertifikat verwendet werden, das auf Ihrem Cluster installiert ist:

```powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

Die resultierende Base64-Zeichenfolge enthält sowohl den Chiffretext des geheimen Werts als auch Informationen zum Zertifikat, das für die Verschlüsselung verwendet wurde.  Die Base64-codierte Zeichenfolge kann in einen Parameter Ihrer Konfigurationsdatei „Settings.xml“ eingefügt werden, wobei das Attribut `IsEncrypted` auf `true` festgelegt wird:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </Section>
</Settings>
```

### <a name="inject-application-secrets-into-application-instances"></a>Einfügen von Geheimnissen aus Anwendungen in Anwendungsinstanzen
Die Bereitstellung in anderen Umgebungen sollte idealerweise so automatisiert wie möglich erfolgen. Zu diesem Zweck können Sie die Geheimnisse in einer Buildumgebung verschlüsseln und die verschlüsselten Geheimnisse beim Erstellen von Anwendungsinstanzen als Parameter angeben.

#### <a name="use-overridable-parameters-in-settingsxml"></a>Verwenden von überschreibbaren Parametern in „Settings.xml“
Die Konfigurationsdatei „Settings.xml“ ermöglicht die Verwendung überschreibbarer Parameter, die bei der Anwendungserstellung angegeben werden können. Verwenden Sie das Attribut `MustOverride` , anstatt einen Wert für einen Parameter anzugeben:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="" MustOverride="true" />
  </Section>
</Settings>
```

Um Werte in „Settings.xml“ zu überschreiben, deklarieren Sie einen Außerkraftsetzungsparameter für den Dienst in „ApplicationManifest.xml“:

```xml
<ApplicationManifest ... >
  <Parameters>
    <Parameter Name="MySecret" DefaultValue="" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides>
      <ConfigOverride Name="Config">
        <Settings>
          <Section Name="MySettings">
            <Parameter Name="MySecret" Value="[MySecret]" IsEncrypted="true" />
          </Section>
        </Settings>
      </ConfigOverride>
    </ConfigOverrides>
  </ServiceManifestImport>
 ```

Der Wert kann nun beim Erstellen einer Instanz der Anwendung als *Anwendungsparameter* angegeben werden. Sie können mithilfe von PowerShell ein Skript zum Erstellen einer Anwendungsinstanz erstellen. Alternativ können Sie C# verwenden, um eine problemlose Integration in einen Erstellungsprozess zu ermöglichen.

Bei Verwendung von PowerShell wird der Parameter als [Hashtabelle](https://technet.microsoft.com/library/ee692803.aspx) an den Befehl `New-ServiceFabricApplication` übergeben:

```powershell
PS C:\Users\vturecek> New-ServiceFabricApplication -ApplicationName fabric:/MyApp -ApplicationTypeName MyAppType -ApplicationTypeVersion 1.0.0 -ApplicationParameter @{"MySecret" = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM="}
```

Bei Verwendung von C# werden Anwendungsparameter in einer `ApplicationDescription` als `NameValueCollection` angegeben:

```csharp
FabricClient fabricClient = new FabricClient();

NameValueCollection applicationParameters = new NameValueCollection();
applicationParameters["MySecret"] = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=";

ApplicationDescription applicationDescription = new ApplicationDescription(
    applicationName: new Uri("fabric:/MyApp"),
    applicationTypeName: "MyAppType",
    applicationTypeVersion: "1.0.0",
    applicationParameters: applicationParameters)
);

await fabricClient.ApplicationManager.CreateApplicationAsync(applicationDescription);
```

## <a name="decrypt-secrets-from-service-code"></a>Entschlüsseln von Geheimnissen aus dem Dienstcode
Dienste in Service Fabric werden unter Windows standardmäßig unter dem Konto NETZWERKDIENST ausgeführt und können ohne weitere Einrichtungsschritte nicht auf Zertifikate zugreifen, die auf dem Knoten installiert sind.

Bei Verwendung eines Datenverschlüsselungszertifikats müssen Sie sicherstellen, dass NETZWERKDIENST bzw. das Benutzerkonto, unter dem der Dienst ausgeführt wird, auf den privaten Schlüssel des Zertifikats zugreifen kann. Bei entsprechender Konfiguration gewährt Service Fabric automatisch Zugriff für Ihren Dienst. Diese Konfiguration kann in der Datei „ApplicationManifest.xml“ vorgenommen werden, indem Sie Benutzer und Sicherheitsrichtlinien für Zertifikate definieren. Im folgenden Beispiel erhält das Konto NETZWERKDIENST Lesezugriff auf ein Zertifikat, das durch seinen Fingerabdruck definiert ist:

```xml
<ApplicationManifest … >
    <Principals>
        <Users>
            <User Name="Service1" AccountType="NetworkService" />
        </Users>
    </Principals>
  <Policies>
    <SecurityAccessPolicies>
      <SecurityAccessPolicy GrantRights=”Read” PrincipalRef="Service1" ResourceRef="MyCert" ResourceType="Certificate"/>
    </SecurityAccessPolicies>
  </Policies>
  <Certificates>
    <SecretsCertificate Name="MyCert" X509FindType="FindByThumbprint" X509FindValue="[YourCertThumbrint]"/>
  </Certificates>
</ApplicationManifest>
```

> [!NOTE]
> Beim Kopieren eines Zertifikatfingerabdrucks aus dem Windows-Snap-In „Zertifikatspeicher“ wird am Anfang der Fingerabdruckzeichenfolge ein unsichtbares Zeichen eingefügt. Dieses unsichtbare Zeichen kann einen Fehler verursachen, wenn Sie versuchen, basierend auf dem Fingerabdruck nach einem Zertifikat zu suchen. Stellen Sie also sicher, dass Sie dieses zusätzliche Zeichen löschen.
> 
> 

### <a name="use-application-secrets-in-service-code"></a>Verwenden von Geheimnissen aus Anwendungen in Dienstcode
Die API für den Zugriff auf Konfigurationswerte in der Datei „Settings.xml“ in einem Konfigurationspaket ermöglicht eine problemlose Entschlüsselung von Werten, bei denen das Attribut `IsEncrypted` auf `true` festgelegt ist. Da der verschlüsselte Text Informationen zum Zertifikat umfasst, das für die Verschlüsselung verwendet wurde, müssen Sie nicht manuell nach dem Zertifikat suchen. Das Zertifikat muss lediglich auf dem Knoten installiert sein, auf dem der Dienst ausgeführt wird. Rufen Sie ganz einfach die Methode `DecryptValue()` auf, um den ursprünglichen geheimen Wert abzurufen:

```csharp
ConfigurationPackage configPackage = this.Context.CodePackageActivationContext.GetConfigurationPackageObject("Config");
SecureString mySecretValue = configPackage.Settings.Sections["MySettings"].Parameters["MySecret"].DecryptValue()
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum [Ausführen von Anwendungen mit unterschiedlichen Sicherheitsberechtigungen](service-fabric-application-runas-security.md)

<!-- Links -->
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[config-package]: service-fabric-application-model.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md

<!-- Images -->
[overview]:./media/service-fabric-application-secret-management/overview.png

