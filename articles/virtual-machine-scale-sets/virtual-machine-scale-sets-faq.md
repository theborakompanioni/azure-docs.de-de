---
title: "Häufig gestellte Fragen zu Azure-VM-Skalierungsgruppen | Microsoft-Dokumentation"
description: "Hier erhalten Sie Antworten auf häufig gestellte Fragen zu VM-Skalierungsgruppen."
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/17/2017
ms.author: negat
ms.custom: na
translationtype: Human Translation
ms.sourcegitcommit: 6e0ad6b5bec11c5197dd7bded64168a1b8cc2fdd
ms.openlocfilehash: e207ace4eb5722e08f2020078dfea9129ef1deb8
ms.lasthandoff: 03/28/2017


---

# <a name="azure-virtual-machine-scale-sets-faqs"></a>Häufig gestellte Fragen zu Azure-VM-Skalierungsgruppen

Hier erhalten Sie Antworten auf häufig gestellte Fragen zu VM-Skalierungsgruppen in Azure.

## <a name="autoscale"></a>Automatische Skalierung

### <a name="what-are-best-practices-for-azure-autoscale"></a>Was sind die bewährten Methoden für die automatische Skalierung in Azure?

Bewährte Methoden für die automatische Skalierung finden Sie unter [Empfohlene Methoden für die automatische Skalierung von VMs, VM-Skalierungsgruppen und Clouddiensten](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-best-practices).

### <a name="where-do-i-find-metric-names-for-autoscaling-that-uses-host-based-metrics"></a>Wo finde ich Metriknamen für die automatische Skalierung mit hostbasierten Metriken?

Metriknamen für die automatische Skalierung mit hostbasierten Metriken finden Sie unter [Unterstützte Metriken von Azure Monitor](https://azure.microsoft.com/documentation/articles/monitoring-supported-metrics/).

### <a name="are-there-any-examples-of-autoscaling-based-on-an-azure-service-bus-topic-and-queue-length"></a>Gibt es Beispiele für die automatische Skalierung auf der Grundlage eines Azure Service Bus-Themas und der Warteschlangenlänge?

Ja. Beispiele für die automatische Skalierung auf der Grundlage eines Azure Service Bus-Themas und der Warteschlangenlänge finden Sie unter [Allgemeine Metriken für die automatische Skalierung in Azure Monitor](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/).

Verwenden Sie für eine Service Bus-Warteschlange den folgenden JSON-Code:

```json
"metricName": "MessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

Verwenden Sie für eine Speicherwarteschlange den folgenden JSON-Code:

```json
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

Ersetzen Sie die Beispielwerte durch die URIs (Uniform Resource Identifiers) Ihrer Ressource.


### <a name="should-i-autoscale-by-using-host-based-metrics-or-a-diagnostics-extension"></a>Soll ich für die automatische Skalierung hostbasierte Metriken oder eine Diagnoseerweiterung verwenden?

Sie können eine Einstellung zur automatischen Skalierung auf einem virtuellen Computer erstellen, um Metriken zu verwenden, die auf der Hostebene oder auf dem Gastbetriebssystem basieren.

Eine Liste mit unterstützten Metriken finden Sie unter [Allgemeine Metriken für die automatische Skalierung in Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-common-metrics). 

Ein vollständiges Beispiel für VM-Skalierungsgruppen finden Sie unter [Konfiguration der erweiterten automatischen Skalierung mithilfe von Resource Manager-Vorlagen für VM-Skalierungsgruppen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-advanced-autoscale-virtual-machine-scale-sets). 

In dem Beispiel werden die CPU-Metrik auf der Hostebene und die Metrik für die Nachrichtenanzahl verwendet.



### <a name="how-do-i-set-alert-rules-on-a-virtual-machine-scale-set"></a>Wie lege ich Warnungsregeln für eine VM-Skalierungsgruppe fest?

Metrikwarnungen für VM-Skalierungsgruppen können über PowerShell oder über die Azure-Befehlszeilenschnittstelle erstellt werden. Weitere Informationen finden Sie unter [Azure Monitor – PowerShell-Schnellstartbeispiele](https://azure.microsoft.com/documentation/articles/insights-powershell-samples/#create-alert-rules) bzw. unter [Azure Monitor – Schnellstartbeispiele für die plattformübergreifende Befehlszeilenschnittstelle](https://azure.microsoft.com/documentation/articles/insights-cli-samples/#work-with-alerts).

Die Zielressourcen-ID (TargetResourceId) der VM-Skalierungsgruppe sieht wie folgt aus: 

/subscriptions/IhreAbonnementID/resourceGroups/<Ihre Ressourcengruppe>/providers/Microsoft.Compute/virtualMachineScaleSets/<Name Ihrer VM-Skalierungsgruppe>

Als Metrik, für die eine Warnung festgelegt werden soll, können Sie einen beliebigen VM-Leistungsindikator auswählen. Weitere Informationen finden Sie im Artikel [Allgemeine Metriken für die automatische Skalierung in Azure Monitor](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/) unter [Gastbetriebssystem-Metriken für Resource Manager-basierte virtuelle Windows-Computer](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-resource-manager-based-windows-vms) und [Gastbetriebssystem-Metriken für virtuelle Linux-Computer](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-linux-vms).

### <a name="how-do-i-set-up-autoscale-on-a-virtual-machine-scale-set-by-using-powershell"></a>Wie richte ich die automatische Skalierung für eine VM-Skalierungsgruppe mithilfe von PowerShell ein?

Informationen zum Einrichten der automatischen Skalierung für eine VM-Skalierungsgruppe mithilfe von PowerShell finden Sie im Blogbeitrag [How to add autoscale to an Azure VM scale set](https://msftstack.wordpress.com/2017/03/05/how-to-add-autoscale-to-an-azure-vm-scale-set/) (Hinzufügen der automatischen Skalierung zu einer Azure-VM-Skalierungsgruppe).




## <a name="certificates"></a>Zertifikate

### <a name="how-do-i-securely-ship-a-certificate-to-the-vm-how-do-i-provision-a-virtual-machine-scale-set-to-run-a-website-where-the-ssl-for-the-website-is-shipped-securely-from-a-certificate-configuration-the-common-certificate-rotation-operation-would-be-almost-the-same-as-a-configuration-update-operation-do-you-have-an-example-of-how-to-do-this"></a>Wie übertrage ich ein Zertifikat sicher auf den virtuellen Computer? Wie stelle ich eine VM-Skalierungsgruppe bereit, um eine Website auszuführen, sodass SSL für die Website sicher aus einer Zertifikatkonfiguration übertragen wird? (Der normale Zertifikatwechsel wäre fast mit einer Konfigurationsaktualisierung gleichzusetzen.) Steht hierfür ein Beispiel zur Verfügung? 

Um ein Zertifikat sicher auf den virtuellen Computer zu übertragen, können Sie ein Kundenzertifikat aus dem Schlüsseltresor des Kunden direkt in einem Windows-Zertifikatspeicher installieren.

Verwenden Sie dazu den folgenden JSON-Code:

```json
        "secrets": [ {
              "sourceVault": {
                      "id": "/subscriptions/{subscriptionid}/resourceGroups/myrg1/providers/Microsoft.KeyVault/vaults/mykeyvault1"
          },
          "vaultCertificates": [ {
                      "certificateUrl": "https://mykeyvault1.vault.azure.net/secrets/{secretname}/{secret-version}",
                  "certificateStore": "certificateStoreName"
          } ]
        } ]
```

Der Code ist für Windows und Linux geeignet.

Weitere Informationen finden Sie unter [Create or update a set](https://msdn.microsoft.com/library/mt589035.aspx) (Erstellen oder Aktualisieren einer Gruppe).


### <a name="example-of-self-signed-certificate"></a>Beispiel für ein selbstsigniertes Zertifikat

1.  Erstellen Sie ein selbstsigniertes Zertifikat in einem Schlüsseltresor.

    Verwenden Sie die folgenden PowerShell-Befehle:

  ```powershell
  Import-Module "C:\Users\mikhegn\Downloads\Service-Fabric-master\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

  Login-AzureRmAccount

  Invoke-AddCertToKeyVault -SubscriptionId <Your SubID> -ResourceGroupName KeyVault -Location westus -VaultName MikhegnVault -CertificateName VMSSCert -Password VmssCert -CreateSelfSignedCertificate -DnsName vmss.mikhegn.azure.com -OutputPath c:\users\mikhegn\desktop\
  ```

  Mit diesem Befehl erhalten Sie die Eingabe für die Azure Resource Manager-Vorlage.

  Ein Beispiel für die Erstellung eines selbstsignierten Zertifikats in einem Schlüsseltresor finden Sie unter [Szenarien für die Clustersicherheit in Service Fabric](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/).

2.  Ändern Sie die Resource Manager-Vorlage.

  Fügen Sie **virtualMachineProfile** die folgende Eigenschaft als Teil der VM-Skalierungsgruppenressource hinzu:

  ```json 
  "osProfile": {
              "computerNamePrefix": "[variables('namingInfix')]",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]",
              "secrets": [
                {
                  "sourceVault": {
                    "id": "[resourceId('KeyVault', 'Microsoft.KeyVault/vaults', 'MikhegnVault')]"
                  },
                  "vaultCertificates": [
                    {
                      "certificateUrl": "https://mikhegnvault.vault.azure.net:443/secrets/VMSSCert/20709ca8faee4abb84bc6f4611b088a4",
                      "certificateStore": "My"
                    }
                  ]
                }
              ]
            }
  ```
  

### <a name="can-i-specify-an-ssh-key-pair-to-use-for-ssh-authentication-with-a-linux-virtual-machine-scale-set-from-a-resource-manager-template"></a>Kann ich ein SSH-Schlüsselpaar für die SSH-Authentifizierung mit einer Linux-VM-Skalierungsgruppe über eine Resource Manager-Vorlage angeben?  

Ja. Die REST-API für **osProfile** gleicht der standardmäßigen VM-REST-API. 

Schließen Sie **osProfile** in Ihre Vorlage ein:

```json 
"osProfile": {
          "computerName": "[variables('vmName')]",
          "adminUsername": "[parameters('adminUserName')]",
          "linuxConfiguration": {
            "disablePasswordAuthentication": "true",
            "ssh": {
              "publicKeys": [
                {
                  "path": "[variables('sshKeyPath')]",
                  "keyData": "[parameters('sshKeyData')]"
                }
              ]
            }
          }
        }
```
 
Dieser JSON-Block wird in der  [GitHub-Schnellstartvorlage „101-vm-sshkey“](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json) verwendet.
 
Das Betriebssystemprofil wird auch in der [GitHub-Schnellstartvorlage „grelayhost.json“](https://github.com/ExchMaster/gadgetron/blob/master/Gadgetron/Templates/grelayhost.json) verwendet.

Weitere Informationen finden Sie unter [Create or update a set](https://msdn.microsoft.com/library/azure/mt589035.aspx#linuxconfiguration) (Erstellen oder Aktualisieren einer Gruppe).
  

### <a name="how-do-i-remove-deprecated-certificates"></a>Wie entferne ich veraltete Zertifikate? 

Ein veraltetes Zertifikat können Sie entfernen, indem Sie es aus der Liste mit den Tresorzertifikaten entfernen. Belassen Sie alle Zertifikate, die auf dem Computer bleiben sollen, in der Liste. Dadurch wird das Zertifikat nicht von allen Ihren virtuellen Computern entfernt. Auch wird das Zertifikat nicht zu neuen virtuellen Computern hinzugefügt, die in der VM-Skalierungsgruppe erstellt werden. 

Wenn Sie das Zertifikat von vorhandenen virtuellen Computern entfernen möchten, schreiben Sie eine benutzerdefinierte Skripterweiterung, die die Zertifikate manuell aus Ihrem Zertifikatspeicher entfernt.
 
### <a name="how-do-i-inject-an-existing-ssh-public-key-into-the-virtual-machine-scale-set-ssh-layer-during-provisioning-i-want-to-store-the-ssh-public-key-values-in-azure-key-vault-and-then-use-them-in-my-resource-manager-template"></a>Wie kann ich einen vorhandenen öffentlichen SSH-Schlüssel während der Bereitstellung in die SSH-Schicht der VM-Skalierungsgruppe einfügen? Ich möchte die Werte des öffentlichen SSH-Schlüssels in Azure Key Vault speichern und dann in der Resource Manager-Vorlage nutzen.

Wenn Sie für die virtuellen Computer nur mit einem öffentlichen SSH-Schlüssel bereitstellen, müssen Sie die öffentlichen Schlüssel nicht in Key Vault speichern. Öffentliche Schlüssel sind nicht geheim.
 
Sie können öffentliche SSH-Schlüssel bei der Erstellung eines virtuellen Linux-Computers im Nur-Text-Format angeben:

```json
"linuxConfiguration": {  
          "ssh": {  
            "publicKeys": [ {  
              "path": "path",
              "keyData": "publickey"
            } ]
          }
```
 
linuxConfiguration-Elementname | Erforderlich | Typ | Beschreibung
--- | --- | --- | --- |  ---
ssh | Nein | Sammlung | Gibt die SSH-Schlüsselkonfiguration für ein Linux-Betriebssystem an.
path | Ja | String | Gibt den Linux-Dateipfad für die SSH-Schlüssel oder das Zertifikat an.
keyData | Ja | String | Gibt einen Base64-codierten öffentlichen SSH-Schlüssel an.

Ein Beispiel finden Sie in der [GitHub-Schnellstartvorlage „101-vm-sshkey“](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).

 
### <a name="when-i-run-update-azurermvmss-after-adding-more-than-one-certificate-from-the-same-key-vault-i-see-the-following-message"></a>Wenn ich `Update-AzureRmVmss` ausführe, nachdem ich mehrere Zertifikate aus dem gleichen Schlüsseltresor hinzugefügt habe, erhalte ich die folgende Meldung:
 
  Update-AzureRmVmss: Die Liste „secret“ enthält wiederholte Instanzen von „/subscriptions/<Meine Abonnement-ID>/resourceGroups/internal-rg-dev/providers/Microsoft.KeyVault/vaults/internal-keyvault-dev“, dies ist unzulässig.
 
Dieser Fall kann eintreten, wenn Sie versuchen, den gleichen Tresor erneut hinzuzufügen, anstatt ein neues Zertifikat für den vorhandenen Quelltresor zu verwenden. Die Befehl `Add-AzureRmVmssSecret` funktioniert nicht ordnungsgemäß, wenn Sie zusätzliche Geheimnisse hinzufügen.
 
Aktualisieren Sie die Liste „$vmss.properties.osProfile.secrets[0].vaultCertificates“, um weitere Geheimnisse aus dem gleichen Schlüsseltresor hinzuzufügen.
 
Die erwartete Eingabestruktur finden Sie unter [Create or update a set](https://msdn.microsoft.com/library/azure/mt589035.aspx) (Erstellen oder Aktualisieren einer Gruppe).
 
Suchen Sie im Schlüsseltresor das Geheimnis im VM-Skalierungsgruppenobjekt. Fügen Sie anschließend Ihren Zertifikatsverweis (URL und Name des Geheimnisspeichers) der dem Tresor zugeordneten Liste hinzu.

> [!NOTE] 
> Derzeit können Zertifikate nicht mithilfe der VM-Skalierungsgruppen-API von virtuellen Computern entfernt werden.
>

Neue virtuelle Computer verfügen nicht über das alte Zertifikat. Virtuelle Computer, die über das Zertifikat verfügen und bereits bereitgestellt wurden, besitzen allerdings das alte Zertifikat.
 
### <a name="can-i-push-certificates-to-the-virtual-machine-scale-set-without-providing-the-password-when-the-certificate-is-in-the-secret-store"></a>Kann ich Zertifikate mithilfe von Push ohne Angabe des Kennworts an die VM-Skalierungsgruppe übertragen, wenn sich das Zertifikat im Geheimnisspeicher befindet?

In Skripts müssen keine hartcodierten Kennwörter verwendet werden. Sie können dynamisch Kennwörter mit den Berechtigungen abrufen, die Sie zum Ausführen des Bereitstellungsskripts verwenden. Wenn Sie über ein Skript verfügen, das ein Zertifikat aus dem Geheimnisspeicher-Schlüsseltresor verschiebt, gibt der Geheimnisspeicherbefehl `get certificate` auch das Kennwort der PFX-Datei aus.
 
### <a name="how-does-the-secrets-property-of-virtualmachineprofileosprofile-for-a-virtual-machine-scale-set-work-why-do-i-need-the-sourcevault-value-when-i-have-to-specify-the-absolute-uri-for-a-certificate-by-using-the-certificateurl-property"></a>Wie funktioniert die Secrets-Eigenschaft von „virtualMachineProfile.osProfile“ für eine VM-Skalierungsgruppe? Warum benötige ich den sourceVault-Wert, wenn ich den absoluten URI für ein Zertifikat mithilfe der certificateUrl-Eigenschaft angeben muss? 

In der Secrets-Eigenschaft des Betriebssystemprofils muss ein WinRM-Zertifikatsverweis (Windows Remote Management) vorhanden sein. 

Die Angabe des Quelltresors dient zur Erzwingung von ACL-Richtlinien (Access Control List; Zugriffssteuerungsliste) aus dem Azure Cloud Service-Modell eines Benutzers. Ohne Angabe des Quelltresors könnten Benutzer über einen Anbieter von Computeressourcen (Compute Resource Provider, CRP) auch ohne entsprechende Berechtigungen Geheimnisse in einem Schlüsseltresor bereitstellen oder darauf zugreifen. ACLs sind sogar für nicht vorhandene Ressourcen vorhanden.

Wenn Sie eine falsche Quelltresor-ID und eine gültige Schlüsseltresor-URL angeben, tritt beim Abfragen des Vorgangs ein Fehler auf.
 
### <a name="if-i-add-secrets-to-an-existing-virtual-machine-scale-set-are-the-secrets-injected-into-existing-vms-or-only-into-new-ones"></a>Wenn ich einer vorhandenen VM-Skalierungsgruppe Geheimnisse hinzufüge, werden diese in vorhandene virtuelle Computer eingefügt oder nur in neue? 

Zertifikate werden allen Ihren virtuellen Computern hinzugefügt. Das gilt auch für bereits vorhandene virtuelle Computer. Wenn die upgradePolicy-Eigenschaft Ihrer VM-Skalierungsgruppe auf **Manuell** festgelegt ist, wird das Zertifikat dem virtuellen Computer hinzugefügt, wenn Sie ein manuelles Update auf dem virtuellen Computer ausführen.
 
### <a name="where-do-i-put-certificates-for-linux-vms"></a>Wohin platziere ich Zertifikate für virtuelle Linux-Computer?

Eine Anleitung zum Bereitstellen von Zertifikaten für virtuelle Linux-Computer finden Sie unter [Deploy Certificates to VMs from customer-managed Key Vault](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/) (Bereitstellen von Zertifikaten für virtuelle Computer über eine vom Kunden verwaltete Key Vault-Instanz).
  
### <a name="how-do-i-add-a-new-vault-certificate-to-a-new-certificate-object"></a>Wie füge ich einem neuen Zertifikatobjekt ein neues Tresorzertifikat hinzu?

Sehen Sie sich das folgende PowerShell-Beispiel an, wenn Sie einem vorhandenen Geheimnis ein Tresorzertifikat hinzufügen möchten. Verwenden Sie nur ein einzelnes Geheimnisobjekt.
 
```powershell
$newVaultCertificate = New-AzureRmVmssVaultCertificateConfig -CertificateStore MY -CertificateUrl https://sansunallapps1.vault.azure.net:443/secrets/dg-private-enc/55fa0332edc44a84ad655298905f1809
 
$vmss.VirtualMachineProfile.OsProfile.Secrets[0].VaultCertificates.Add($newVaultCertificate)
 
Update-AzureRmVmss -VirtualMachineScaleSet $vmss -ResourceGroup $rg -Name $vmssName
```
 
### <a name="what-happens-to-certificates-if-you-reimage-a-vm"></a>Was geschieht mit Zertifikaten, wenn Sie für einen virtuellen Computer ein Reimaging durchführen?

Wenn Sie für einen virtuellen Computer ein Reimaging durchführen, werden die Zertifikate gelöscht. Beim Reimaging wird der gesamte Betriebssystemdatenträger gelöscht. 
 
### <a name="what-happens-if-you-delete-a-certificate-from-the-key-vault"></a>Was geschieht, wenn Sie ein Zertifikat aus dem Schlüsseltresor löschen?

Wenn das Geheimnis aus dem Schlüsseltresor gelöscht wird und Sie anschließend für alle Ihre virtuellen Computer `stop deallocate` ausführen und sie dann neu starten, tritt ein Fehler auf. Der Grund für den Fehler: Der CRP muss das Geheimnis aus dem Schlüsseltresor abrufen, das ist aber nicht möglich. In diesem Szenario können Sie die Zertifikate aus dem VM-Skalierungsgruppenmodell löschen. 

Kundengeheimnisse werden von der CRP-Komponente nicht speichert. Wenn Sie `stop deallocate` für alle virtuellen Computer in der VM-Skalierungsgruppe ausführen, wird der Cache gelöscht. In diesem Szenario werden die Geheimnisse aus dem Schlüsseltresor abgerufen.

Beim horizontalen Hochskalieren tritt dieses Problem nicht auf, da eine zwischengespeicherte Kopie des Geheimnisses in Azure Service Fabric (Einzelfabric-Mandantenmodell) vorhanden ist.
 
### <a name="why-do-i-have-to-specify-the-exact-location-for-the-certificate-url-httpsname-of-the-vaultvaultazurenet443secretsexact-location-as-indicated-in-service-fabric-cluster-security-scenarioshttpsazuremicrosoftcomdocumentationarticlesservice-fabric-cluster-security"></a>Warum muss ich den genauen Speicherort für die Zertifikat-URL (https://<name of the vault>.vault.azure.net:443/secrets/<exact location>) angeben, wie unter [Szenarien für die Clustersicherheit in Service Fabric](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/) beschrieben?
 
Laut Azure Key Vault-Dokumentation gibt die REST-API zum Abrufen des Geheimnisses die neueste Version des Geheimnisses zurück, wenn keine Version angegeben ist.
 
Methode | URL
--- | ---
GET | https://mykeyvault.vault.azure.net/secrets/{Name des geheimen Schlüssels}/{Version des geheimen Schlüssels}?api-version={API-Version}

Ersetzen Sie {*Name des geheimen Schlüssels*} durch den Namen und {*Version des geheimen Schlüssels*} durch die Version des Geheimnisses, das Sie abrufen möchten. Die Geheimnisversion kann weggelassen werden. In diesem Fall wird die aktuelle Version abgerufen.
  
### <a name="why-do-i-have-to-specify-the-certificate-version-when-i-use-key-vault"></a>Warum muss ich bei Verwendung von Key Vault die Zertifikatversion angeben?

Die Angabe der Zertifikatversion ist bei Verwendung von Key Vault erforderlich, um dem Benutzer gegenüber deutlich zu machen, welches Zertifikat auf den virtuellen Computern bereitgestellt wird.

Wenn Sie einen virtuellen Computer erstellen und anschließend Ihr Geheimnis im Schlüsseltresor aktualisieren, wird das neue Zertifikat nicht auf Ihre virtuellen Computer heruntergeladen. Ihre virtuellen Computer scheinen jedoch darauf zu verweisen, und neue virtuelle Computer erhalten das neue Geheimnis. Um dies zu vermeiden, müssen Sie eine Geheimnisversion verweisen.

### <a name="my-team-works-with-several-certificates-that-are-distributed-to-us-as-cer-public-keys-what-is-the-recommended-approach-for-deploying-these-certificates-to-a-virtual-machine-scale-set"></a>Mein Team arbeitet mit mehreren Zertifikaten, die uns als öffentliche CER-Schlüssel zur Verfügung gestellt werden. Wie stelle ich diese Zertifikate am besten für eine VM-Skalierungsgruppe bereit?

Wenn Sie öffentliche CER-Schlüssel für eine VM-Skalierungsgruppe bereitstellen möchten, können Sie eine PFX-Datei erstellen, die nur CER-Dateien enthält. Verwenden Sie hierzu `X509ContentType = Pfx`. Laden Sie beispielsweise die CER-Datei als Objekt vom Typ „x509Certificate2“ in C# oder PowerShell, und rufen Sie dann die Methode auf. 

Weitere Informationen finden Sie unter [X509Certificate.Export Method (X509ContentType, String)](https://msdn.microsoft.com/library/24ww6yzk(v=vs.110.aspx)) (X509Certificate.Export-Methode (X509ContentType, String)).

### <a name="i-do-not-see-an-option-for-users-to-pass-in-certificates-as-base64-strings-most-other-resource-providers-have-this-option"></a>Ich sehe keine Option, mit der Benutzer Zertifikate als Base64-Zeichenfolgen übergeben können. Bei den meisten anderen Ressourcenanbietern steht eine solche Option zur Verfügung.

Sie können das Übergeben eines Zertifikats als Base64-Zeichenfolge emulieren, indem Sie die neueste URL mit Versionsangabe in einer Resource Manager-Vorlage extrahieren. Schließen Sie in Ihre Resource Manager-Vorlage die folgende JSON-Eigenschaft ein:

```json 
"certificateUrl": "[reference(resourceId(parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults/secrets', parameters('vaultName'), parameters('secretName')), '2015-06-01').secretUriWithVersion]"
```
 
### <a name="do-i-have-to-wrap-certificates-in-json-objects-in-key-vaults"></a>Muss ich Zertifikate in Schlüsseltresoren mit JSON-Objekten umschließen?

In VM-Skalierungsgruppen und auf virtuellen Computern müssen Zertifikate mit JSON-Objekten umschlossen werden. 

Wir unterstützen auch den Inhaltstyp „application/x-pkcs12“. Eine Anleitung zur Verwendung von „application/x-pkcs12“ finden Sie unter [PFX Certificate in Azure Key Vault](http://www.rahulpnath.com/blog/pfx-certificate-in-azure-key-vault/) (PFX-Zertifikat in Azure Key Vault).
 
CER-Dateien werden derzeit nicht unterstützt. Zu verwendende CER-Dateien müssen in PFX-Container exportiert werden.



## <a name="compliance"></a>Compliance

### <a name="are-virtual-machine-scale-sets-pci-compliant"></a>Sind VM-Skalierungsgruppen PCI-konform?

Bei VM-Skalierungsgruppen handelt es sich um eine dünne API-Schicht über dem CRP. Beide Komponenten sind Teil der Computeplattform in der Azure-Dienststruktur.

VM-Skalierungsgruppen sind hinsichtlich der Konformität ein grundlegender Bestandteil der Azure-Computeplattform. Sie teilen sich Team, Tools, Prozesse, Bereitstellungsmethode, Sicherheitskontrollen, JIT-Kompilierung, Überwachung, Warnungen usw. mit dem CRP. VM-Skalierungsgruppen sind PCI-konform (Payment Card Industry), da der CRP Teil des aktuellen PCI DSS-Nachweises (Data Security Standard) ist.

Weitere Informationen finden Sie im [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/Compliance/PCI).






## <a name="extensions"></a>Erweiterungen

### <a name="how-do-i-delete-a-virtual-machine-scale-set-extension"></a>Wie lösche ich eine VM-Skalierungsgruppenerweiterung?

Verwenden Sie zum Löschen einer VM-Skalierungsgruppenerweiterung das folgende PowerShell-Beispiel:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" 

$vmss=Remove-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "extensionName"

Update-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" -VirtualMacineScaleSet $vmss
```
 
Den Wert für „extensionName“ finden Sie in `$vmss`.
   
### <a name="is-there-a-virtual-machine-scale-set-template-example-that-integrates-with-operations-management-suite"></a>Gibt es ein Beispiel für eine VM-Skalierungsgruppenvorlage mit Operations Management Suite-Integration?

Ein Beispiel für eine VM-Skalierungsgruppenvorlage mit Operations Management Suite-Integration finden Sie unter [Deploy Azure Service Fabric Cluster and enable monitoring using OMS Log Analytics](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/ServiceFabric) (Bereitstellen eines Azure Service Fabric-Clusters und Aktivieren der Überwachung mithilfe von Log Analytics) im zweiten Beispiel.
   
### <a name="extensions-seem-to-run-in-parallel-on-virtual-machine-scale-sets-this-causes-my-custom-script-extension-to-fail-what-can-i-do-to-fix-this"></a>Erweiterungen scheinen in VM-Skalierungsgruppen parallel ausgeführt zu werden. Das führt zu einem Fehler bei meiner benutzerdefinierten Skripterweiterung. Wie kann ich dieses Problem beheben?

Informationen zur Erweiterungssequenzierung in VM-Skalierungsgruppen finden Sie unter [Extension sequencing in Azure VM Scale Sets](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/) (Erweiterungssequenzierung in Azure-VM-Skalierungsgruppen).
 
 
### <a name="how-do-i-reset-the-password-for-vms-in-my-virtual-machine-scale-set"></a>Wie setze ich das Kennwort für virtuelle Computer in meiner VM-Skalierungsgruppe zurück?

Das Kennwort für virtuelle Computer in Ihrer VM-Skalierungsgruppe können Sie mithilfe von Zugriffserweiterungen für virtuelle Computer zurücksetzen. 

Verwenden Sie das folgende PowerShell-Beispiel:

```powershell
$vmssName = "myvmss"
$vmssResourceGroup = "myvmssrg"
$publicConfig = @{"UserName" = "newuser"}
$privateConfig = @{"Password" = "********"}
 
$extName = "VMAccessAgent"
$publisher = "Microsoft.Compute"
$vmss = Get-AzureRmVmss -ResourceGroupName $vmssResourceGroup -VMScaleSetName $vmssName
$vmss = Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name $extName -Publisher $publisher -Setting $publicConfig -ProtectedSetting $privateConfig -Type $extName -TypeHandlerVersion "2.0" -AutoUpgradeMinorVersion $true
Update-AzureRmVmss -ResourceGroupName $vmssResourceGroup -Name $vmssName -VirtualMachineScaleSet $vmss
```
 
 
### <a name="how-do-i-add-an-extension-to-all-vms-in-my-virtual-machine-scale-set"></a>Wie füge ich allen virtuellen Computern in meiner VM-Skalierungsgruppe eine Erweiterung hinzu?

Wenn die Aktualisierungsrichtlinie auf **Automatisch** festgelegt ist, werden durch die erneute Bereitstellung der Vorlage mit den neuen Erweiterungseigenschaften alle virtuellen Computer aktualisiert.

Wenn die Aktualisierungsrichtlinie auf **Manuell** festgelegt ist, aktualisieren Sie zunächst die Erweiterung, und aktualisieren Sie anschließend manuell alle Instanzen auf Ihren virtuellen Computern.

  
### <a name="if-the-extensions-associated-with-an-existing-virtual-machine-scale-set-are-updated-are-existing-vms-affected-that-is-will-the-vms-not-match-the-virtual-machine-scale-set-model-or-are-they-ignored-when-an-existing-machine-is-service-healed-or-reimaged-are-the-scripts-that-are-currently-configured-on-the-virtual-machine-scale-set-executed-or-are-the-scripts-that-were-configured-when-the-vm-was-first-created-used"></a>Hat die Aktualisierung der Erweiterungen, die einer vorhandenen VM-Skalierungsgruppe zugeordnet sind, Auswirkungen auf bereits vorhandene virtuelle Computer? (Anders gefragt: Entsprechen die virtuellen Computer dann *nicht* dem virtuellen Modell der VM-Skalierungsgruppe?) Oder werden sie ignoriert? Wenn ein vorhandener Computer vom Dienst repariert wird oder ein Reimaging durchgeführt wird, werden dann die Skripts ausgeführt, die gegenwärtig für die VM-Skalierungsgruppe konfiguriert sind, oder werden die Skripts verwendet, die bei der Erstellung des virtuellen Computers konfiguriert wurden?

Wenn die Erweiterungsdefinition im VM-Skalierungsgruppenmodell aktualisiert wird und die upgradePolicy-Eigenschaft auf **Automatisch** festgelegt ist, werden die virtuellen Computer aktualisiert. Ist die upgradePolicy-Eigenschaft auf **Manuell** festgelegt, werden Erweiterungen als nicht dem Modell entsprechend gekennzeichnet. 

Wenn ein vorhandener virtueller Computer vom Dienst repariert wird, wird das wie ein Neustart behandelt, und die Erweiterungen werden nicht erneut ausgeführt. Wird ein Reimaging durchgeführt, ist das mit dem Austauschen des Betriebssystemdatenträgers mit dem Quellimage vergleichbar. Sämtliche Spezialisierungen aus dem aktuellen Modell (etwa Erweiterungen) werden ausgeführt.
 
### <a name="how-do-i-join-a-virtual-machine-scale-set-to-an-azure-ad-domain"></a>Wie füge ich eine VM-Skalierungsgruppe einer Azure AD-Domäne hinzu?

Eine VM-Skalierungsgruppe kann einer Azure AD-Domäne (Azure Active Directory) durch Definieren einer Erweiterung hinzugefügt werden. 

Verwenden Sie zum Definieren einer Erweiterung die JsonADDomainExtension-Eigenschaft:

```json
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "joindomain",
                                "properties": {
                                    "publisher": "Microsoft.Compute",
                                    "type": "JsonADDomainExtension",
                                    "typeHandlerVersion": "1.0",
                                    "settings": {
                                        "Name": "[parameters('domainName')]",
                                        "OUPath": "[variables('ouPath')]",
                                        "User": "[variables('domainAndUsername')]",
                                        "Restart": "true",
                                        "Options": "[variables('domainJoinOptions')]"
                                    },
                                    "protectedsettings": {
                                        "Password": "[parameters('domainJoinPassword')]"
                                    }
                                }
                            }
                        ]
                    }
```
 
### <a name="my-virtual-machine-scale-set-extension-is-trying-to-install-something-that-requires-a-reboot-for-example-commandtoexecute-powershellexe--executionpolicy-unrestricted-install-windowsfeature-name-fs-resource-manager-includemanagementtools"></a>Meine VM-Skalierungsgruppenerweiterung versucht, etwas installieren, das einen Neustart erfordert. Beispiel: "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted Install-WindowsFeature –Name FS-Resource-Manager –IncludeManagementTools"

Wenn Ihre VM-Skalierungsgruppenerweiterung versucht, etwas installieren, das einen Neustart erfordert, können Sie die Erweiterung „Azure Automation Desired State Configuration (Automation DSC)“ verwenden. Bei Verwendung des Betriebssystems Windows Server 2012 R2 führt Azure das WMF 5.0-Setup (Windows Management Framework) aus, startet neu und setzt dann die Konfiguration fort. 
 
### <a name="how-do-i-turn-on-antimalware-in-my-virtual-machine-scale-set"></a>Wie aktiviere ich Antischadsoftware in meiner VM-Skalierungsgruppe?

Antischadsoftware können Sie in Ihrer VM-Skalierungsgruppe mithilfe des folgenden PowerShell-Beispiels aktiveren:

```powershell
$rgname = 'autolap'
$vmssname = 'autolapbr'
$location = 'eastus'
 
# Retrieve the most recent version number of the extension.
$allVersions= (Get-AzureRmVMExtensionImage -Location $location -PublisherName "Microsoft.Azure.Security" -Type "IaaSAntimalware").Version
$versionString = $allVersions[($allVersions.count)-1].Split(".")[0] + "." + $allVersions[($allVersions.count)-1].Split(".")[1]
 
$VMSS = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname
echo $VMSS
Add-AzureRmVmssExtension -VirtualMachineScaleSet $VMSS -Name "IaaSAntimalware" -Publisher "Microsoft.Azure.Security" -Type "IaaSAntimalware" -TypeHandlerVersion $versionString
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $VMSS 
```

### <a name="i-need-to-execute-a-custom-script-thats-hosted-in-a-private-storage-account-the-script-runs-successfully-when-the-storage-is-public-but-when-i-try-to-use-a-shared-access-signature-sas-it-fails-this-message-is-displayed-missing-mandatory-parameters-for-valid-shared-access-signature-linksas-works-fine-from-my-local-browser"></a>Ich muss ein benutzerdefiniertes Skript ausführen, das in einem privaten Speicherkonto gehostet wird. Bei Verwendung eines privaten Speichers funktioniert das Skript einwandfrei, wenn ich jedoch eine SAS (Shared Access Signature) verwenden möchte, tritt ein Fehler auf. Folgende Meldung wird angezeigt: „Erforderliche Parameter für gültige SAS (Shared Access Signature) fehlen.“ Link und SAS funktionieren in meinem lokalen Browser problemlos.

Zum Ausführen eines benutzerdefinierten Skripts, das in einem privaten Speicherkonto gehostet wird, müssen Sie geschützte Einstellungen mit dem Speicherkontoschlüssel und -name einrichten. Weitere Informationen finden Sie unter [CustomScript-Erweiterung für Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/#template-example-for-a-windows-vm-with-protected-settings).







## <a name="networking"></a>Netzwerk
 
### <a name="how-do-i-do-a-vip-swap-for-virtual-machine-scale-sets-in-the-same-subscription-and-same-region"></a>Wie führe ich ein VIP-Swap für VM-Skalierungsgruppen im gleichen Abonnement und in der gleichen Region aus?

Informationen zum Ausführen eines VIP-Swaps für VM-Skalierungsgruppen im gleichen Abonnement und in der gleichen Region finden Sie unter [VIP Swap – blue-green deployment in Azure Resource Manager](https://msftstack.wordpress.com/2017/02/24/vip-swap-blue-green-deployment-in-azure-resource-manager/) (VIP-Swap: Blaugrün-Bereitstellung in Azure Resource Manager).
 
  
### <a name="what-is-the-resourceguid-property-on-a-nic-used-for"></a>Wozu dient die resourceGuid-Eigenschaft einer NIC?

Bei der resourceGuid-Eigenschaft einer NIC (Network Interface Card; Netzwerkschnittstellenkarte) handelt es sich um eine eindeutige ID. Diese ID wird von unteren Schichten irgendwann in der Zukunft protokolliert. 
 
### <a name="how-do-i-specify-a-range-of-private-ip-addresses-to-use-for-static-private-ip-address-allocation"></a>Wie gebe ich einen Bereich privater IP-Adressen für die statische private IP-Adresszuordnung an?

IP-Adressen werden in einem von Ihnen angegebenen Subnetz ausgewählt. 

Die IP-Adresszuordnungsmethode für VM-Skalierungsgruppen ist immer dynamisch. Das bedeutet aber nicht, dass sich diese IP-Adressen ändern können. In diesem Fall bedeutet „dynamisch“ lediglich, dass Sie die IP-Adresse nicht in einer PUT-Anforderung angeben. Verwenden Sie zum Angeben der statischen Gruppe das Subnetz. 
    
### <a name="how-do-i-deploy-a-virtual-machine-scale-set-to-an-existing-azure-virtual-network"></a>Wie stelle ich eine VM-Skalierungsgruppe für ein vorhandenes virtuelles Azure-Netzwerk bereit? 

Informationen zum Bereitstellen einer VM-Skalierungsgruppe für ein vorhandenes virtuelles Azure-Netzwerk finden Sie unter [Deploy a VM Scale Set into an existing vnet](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-existing-vnet) (Bereitstellen einer VM-Skalierungsgruppe für ein vorhandenes VNET). 

### <a name="how-do-i-add-the-ip-address-of-the-first-vm-in-a-virtual-machine-scale-set-to-the-output-of-a-template"></a>Wie füge ich die IP-Adresse des ersten virtuellen Computers in einer VM-Skalierungsgruppe der Ausgabe einer Vorlage hinzu?

Informationen zum Hinzufügen der IP-Adresse des ersten virtuellen Computers in einer VM-Skalierungsgruppe zur Ausgabe einer Vorlage finden Sie unter [ARM: Get VMSS's private IPs](http://stackoverflow.com/questions/42790392/arm-get-vmsss-private-ips) (ARM: Abrufen der privaten IP-Adressen einer VMSS).



## <a name="scale"></a>Skalieren

### <a name="in-what-case-would-i-create-a-virtual-machine-scale-set-with-fewer-than-two-vms"></a>In welchem Fall erstelle ich eine VM-Skalierungsgruppe mit weniger als zwei virtuellen Computern?

Ein Grund für die Erstellung einer VM-Skalierungsgruppe mit weniger als zwei virtuellen Computern wäre die Verwendung der elastischen Eigenschaften einer VM-Skalierungsgruppe. So können Sie beispielsweise eine VM-Skalierungsgruppe ohne virtuelle Computer bereitstellen, um Ihre Infrastruktur zu definieren, ohne die laufenden Kosten für virtuelle Computer zu bezahlen. Wenn Sie dann für die Bereitstellung von virtuellen Computern bereit sind, können Sie die Kapazität der VM-Skalierungsgruppe auf die Anzahl von Produktionsinstanzen erhöhen.

Ein weiterer Grund für die Erstellung einer VM-Skalierungsgruppe mit weniger als zwei virtuellen Computern wäre, wenn Sie sich weniger Gedanken um die Verfügbarkeit machen, sondern eher auf die Verwendung einer Verfügbarkeitsgruppe mit diskreten virtuellen Computern Wert legen. VM-Skalierungsgruppen ermöglichen die Verwendung undifferenzierter, austauschbarer Computeeinheiten. Diese Einheitlichkeit ist ein wichtiges Unterscheidungsmerkmal von VM-Skalierungsgruppen im Vergleich zu Verfügbarkeitsgruppen. Bei vielen zustandslosen Workloads werden keine einzelnen Einheiten nachverfolgt. Sie können zentral auf eine einzelne Computeeinheit herunterskaliert werden, wenn die Workload zurückgeht, und werden zentral wieder auf viele Computeeinheiten hochskaliert, wenn sich die Workload erhöht.

### <a name="how-do-i-change-the-number-of-vms-in-a-virtual-machine-scale-set"></a>Wie ändere ich die Anzahl von virtuellen Computern in einer VM-Skalierungsgruppe?

Informationen zum Ändern der Anzahl von virtuellen Computern in einer VM-Skalierungsgruppe finden Sie unter [Change the instance count of an Azure VM Scale Set](https://msftstack.wordpress.com/2016/05/13/change-the-instance-count-of-an-azure-vm-scale-set/) (Ändern der Instanzanzahl einer Azure-VM-Skalierungsgruppe).

### <a name="how-do-i-define-custom-alerts-for-when-certain-thresholds-are-reached"></a>Wie definiere ich benutzerdefinierte Warnungen für das Erreichen bestimmter Schwellenwerte?

Warnungen für angegebene Schwellenwerte können mit einer gewissen Flexibilität gehandhabt werden. So können Sie beispielsweise benutzerdefinierte Webhooks definieren. Das folgende Webhookbeispiel stammt aus einer Resource Manager-Vorlage:

```json
   {
         "type": "Microsoft.Insights/autoscaleSettings",
           "apiVersion": "[variables('insightsApi')]",
                 "name": "autoscale",
                   "location": "[parameters('resourceLocation')]",
                     "dependsOn": [
                         "[concat('Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
                 ],
                 "properties": {
                         "name": "autoscale",
                     "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/',  resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                     "enabled": true,
                     "notifications": [{
                               "operation": "Scale",
                              "email": {
                                     "sendToSubscriptionAdministrator": true,
                                     "sendToSubscriptionCoAdministrators": true,
                                     "customEmails": [
                                        "youremail@address.com"
                                     ]},
                              "webhooks": [{
                                    "serviceUri": "https://events.pagerduty.com/integration/0b75b57246814149b4d87fa6e1273687/enqueue",
                                    "properties": {
                                        "key1": "custommetric",
                                        "key2": "scalevmss"
                                    }
                                    }
                              ]}],
```

In diesem Beispiel wird bei Erreichen eines Schwellenwerts eine Warnung an „Pagerduty.com“ ausgegeben.



## <a name="troubleshooting"></a>Problembehandlung

### <a name="how-do-i-turn-on-boot-diagnostics"></a>Wie aktiviere ich die Startdiagnose?

Erstellen Sie zum Aktivieren der Startdiagnose zunächst ein Speicherkonto. Fügen Sie dann den folgenden JSON-Block unter **virtualMachineProfile** in die VM-Skalierungsgruppe ein, und aktualisieren Sie die VM-Skalierungsgruppe:

```json
      "diagnosticsProfile": {
        "bootDiagnostics": {
          "enabled": true,
          "storageUri": "http://yourstorageaccount.blob.core.windows.net"
        }
      }
```

Beim Erstellen eines neuen virtuellen Computers zeigt die InstanceView-Eigenschaft des virtuellen Computers die Details für den Screenshot usw. an. Hier sehen Sie ein Beispiel:
 
```json
"bootDiagnostics": {
    "consoleScreenshotBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.screenshot.bmp",
    "serialConsoleLogBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.serialconsole.log"
  }
```

 

## <a name="updates"></a>Aktualisierungen

### <a name="how-to-i-update-my-virtual-machine-scale-set-to-a-new-image-how-do-i-manage-patching"></a>Wie kann ich meine VM-Skalierungsgruppe auf ein neues Image aktualisieren? Wie verwalte das Patchen?

Informationen zum Aktualisieren Ihrer VM-Skalierungsgruppe auf ein neues Image sowie zum Verwalten des Patchens finden Sie unter [Upgraden einer VM-Skalierungsgruppe](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set).

### <a name="can-i-use-the-reimage-operation-to-reset-a-vm-without-changing-the-image-that-is-i-want-reset-a-vm-to-factory-settings-rather-than-to-a-new-image"></a>Kann ich einen virtuellen Computer mittels Reimaging zurücksetzen, ohne das Image zu ändern? (Ich möchte einen virtuellen Computer auf die Werkseinstellungen zurücksetzen, nicht auf ein neues Image.)

Ja. Sie können einen virtuellen Computer mittels Reimaging zurücksetzen, ohne das Image zu ändern. Falls Ihre VM-Skalierungsgruppe allerdings auf ein Plattformimage mit `version = latest` verweist, kann Ihr virtueller Computer auf ein höheres Betriebssystemimage aktualisiert werden, wenn Sie das `reimage` aufrufen.

Weitere Informationen finden Sie unter [Manage all VMs in a set](https://docs.microsoft.com/rest/api/virtualmachinescalesets/manage-all-vms-in-a-set) (Verwalten aller virtuellen Computer in einer Gruppe).



## <a name="virtual-machine-properties"></a>Eigenschaften für virtuelle Computer

### <a name="how-do-i-get-property-information-for-each-vm-without-making-multiple-calls-for-example-how-would-i-get-the-fault-domain-for-each-of-the-100-vms-in-my-virtual-machine-scale-set"></a>Wie erhalte ich Informationen zu den Eigenschaften für die einzelnen virtuellen Computer, ohne mehrere Aufrufe auszuführen? Wie kann ich also beispielsweise die Fehlerdomäne für jeden der 100 virtuellen Computer in meiner VM-Skalierungsgruppe abrufen?

Wenn Sie Informationen zu den Eigenschaften für die einzelnen virtuellen Computer abrufen möchten, ohne mehrere Aufrufe auszuführen, können Sie `ListVMInstanceViews` im Rahmen eines REST-API-Vorgangs vom Typ `GET` für den folgenden Ressourcen-URI aufrufen:

/subscriptions/<Abonnement-ID>/resourceGroups/<Name der Ressourcengruppe>/providers/Microsoft.Compute/virtualMachineScaleSets/<Name der Skalierungsgruppe>/virtualMachines?$expand=instanceView&$select=instanceView

### <a name="can-i-pass-different-extension-arguments-to-different-vms-in-a-virtual-machine-scale-set"></a>Kann ich unterschiedliche Erweiterungsargumente an verschiedene virtuelle Computer in einer VM-Skalierungsgruppe übergeben?

Nein. Es ist nicht möglich, unterschiedliche Erweiterungsargumente an verschiedene virtuelle Computer in einer VM-Skalierungsgruppe zu übergeben. Erweiterungen können aber unter Berücksichtigung der eindeutigen Eigenschaften des ausführenden virtuellen Computers (beispielsweise des Computernamens) aktiv werden. Darüber hinaus können Erweiterungen Instanzmetadaten von „http://169.254.169.254“ abfragen, um weitere Informationen zu dem virtuellen Computer zu erhalten.

### <a name="why-are-there-gaps-between-my-virtual-machine-scale-set-vm-machine-names-and-vm-ids-for-example-0-1-3"></a>Warum gibt es Lücken zwischen den Namen der virtuellen Computer meiner VM-Skalierungsgruppe und den IDs virtueller Computer? Beispiel: 0, 1, 3...

Die Lücken zwischen den Namen der virtuellen Computer Ihrer VM-Skalierungsgruppe und den IDs virtueller Computer sind darauf zurückzuführen, dass die Eigenschaft **overprovision** Ihrer VM-Skalierungsgruppe auf den Standardwert **true** festgelegt ist. Wenn die Überbereitstellung auf **true** festgelegt ist, werden mehr virtuelle Computer als angefordert bereitgestellt. Zusätzliche virtuelle Computer werden anschließend gelöscht. Dies erhöht zwar die Bereitstellungszuverlässigkeit, geht jedoch zulasten fortlaufender Namen und zusammenhängender Regeln für die Netzwerkadressenübersetzung (Network Address Translation, NAT). 

Die Eigenschaft kann auf **false** festgelegt werden. Bei kleinen VM-Skalierungsgruppen hat das keine nennenswerten Auswirkungen auf die Bereitstellungszuverlässigkeit.

### <a name="what-is-the-difference-between-deleting-a-vm-in-a-virtual-machine-scale-set-and-deallocating-the-vm-when-should-i-choose-one-over-the-other"></a>Was ist der Unterschied zwischen dem Löschen eines virtuellen Computers in einer VM-Skalierungsgruppe und dem Aufheben der Zuordnung des virtuellen Computers? Wann sollte ich was wählen?

Der Hauptunterschied zwischen dem Löschen eines virtuellen Computers in einer VM-Skalierungsgruppe und dem Aufheben der Zuordnung des virtuellen Computers besteht darin, dass bei `deallocate` die virtuellen Festplatten (Virtual Hard Disks, VHDs) nicht gelöscht werden. Das Ausführen von `stop deallocate` ist mit Speicherkosten verbunden. Im Anschluss finden Sie Gründe für die Verwendung der einzelnen Verfahren:

- Sie möchten keine Computekosten mehr bezahlen, aber den Datenträgerzustand der virtuellen Computer beibehalten.
- Sie möchten eine Gruppe virtueller Computer schneller starten als sich eine VM-Skalierungsgruppe horizontal hochskalieren lässt.
  - Im Zusammenhang mit diesem Szenario: Sie haben ggf. ein eigenes Modul für die automatische Skalierung erstellt und möchten eine schnellere End-to-End-Skalierung erreichen.
- Sie verfügen über eine VM-Skalierungsgruppe, die ungleichmäßig auf Fehler- oder Updatedomänen verteilt ist. Dieser Fall kann eintreten, wenn Sie selektiv virtuelle Computer gelöscht haben oder virtuelle Computer nach einer Überbereitstellung gelöscht wurden. Wenn Sie `stop deallocate` und anschließend `start` für die VM-Skalierungsgruppe ausführen, werden die virtuellen Computer gleichmäßig auf Fehler- oder Updatedomänen verteilt.


