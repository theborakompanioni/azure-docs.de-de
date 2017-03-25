---
title: "Häufig gestellte Fragen zu Azure-VM-Skalierungsgruppen | Microsoft-Dokumentation"
description: "Hier erhalten Sie Antworten auf häufig gestellte Fragen zu VM-Skalierungsgruppen"
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
ms.date: 3/08/2017
ms.author: negat
ms.custom: na
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: 8d0adfb995ecfb56ff33814e462a88f3d1102d58
ms.lasthandoff: 03/15/2017


---

# <a name="azure-virtual-machine-scale-sets-faq"></a>Häufig gestellte Fragen zu Azure-VM-Skalierungsgruppen

Dieser Artikel enthält Antworten auf häufig gestellte Fragen zu Skalierungsgruppen.

## <a name="autoscale"></a>Autoscale

### <a name="what-are-best-practices-for-azure-autoscale"></a>Was sind die bewährten Methoden für die automatische Skalierung in Azure?

Ja. Siehe: https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-best-practices

### <a name="where-do-i-find-the-metric-names-for-autoscaling-using-host-based-metrics"></a>Wo finde ich die Metriknamen für die automatische Skalierung mithilfe von hostbasierten Metriken?

https://azure.microsoft.com/documentation/articles/monitoring-supported-metrics/

### <a name="are-there-any-examples-of-autoscaling-based-on-a-service-bus-topic-and-queue-length"></a>Gibt es Beispiele für die automatische Skalierung basierend auf einem Service Bus-Thema und der Warteschlangenlänge?

Ja. Siehe:

https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/

Für die Service Bus-Warteschlange:

```json
"metricName": "MessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

Für Speicherwarteschlangen:

```json
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

Ersetzen Sie diese Beispielwerte durch die entsprechenden Ressourcen-URIs.


### <a name="should-we-autoscale-with-host-based-metrics-or-use-a-diagnostics-extension"></a>Sollten wir für die automatische Skalierung hostbasierte Metriken oder eine Diagnoseerweiterung verwenden?

Sie können eine Einstellung zur automatischen Skalierung auf einem virtuellen Computer erstellen, um Metriken auf Hostebene zu verwenden. Sie können aber auch Metriken verwenden, die auf dem Gastbetriebssystem basieren.

Sehen Sie sich diese Liste der unterstützten Metriken an: https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-common-metrics. Dies ist ein vollständiges Codebeispiel für Skalierungsgruppen (in diesem Fall wird die CPU-Metrik auf Hostebene und die Metrik für die Nachrichtenanzahl verwendet):

https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-advanced-autoscale-virtual-machine-scale-sets

### <a name="how-can-i-set-alert-rules-on-a-scale-set"></a>Wie kann ich Warnungsregeln für eine Skalierungsgruppe festlegen?

Sie können Warnungen zu Metriken für eine Skalierungsgruppe über PS oder CLI erstellen. Siehe:

https://azure.microsoft.com/documentation/articles/insights-powershell-samples/#create-alert-rules

https://azure.microsoft.com/documentation/articles/insights-cli-samples/#work-with-alerts

Die TargetResourceId der Skalierungsgruppe sieht wie folgt aus: /subscriptions/IhreAbonnementID/resourceGroups/IhreRessourcengruppe/providers/Microsoft.Compute/virtualMachineScaleSets/NameIhrerVMSkalierungsgruppe

Sie können einen beliebigen VM-Leistungsindikator als Metrik auswählen, für die Warnungen ausgegeben werden sollen:

https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#compute-metrics-for-windows-vm-v2-as-a-guest-os

https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#compute-metrics-for-linux-vm-v2-as-a-guest-os

### <a name="how-can-i-set-up-autoscale-on-a-scale-set-using-powershell"></a>Wie kann ich die automatische Skalierung für eine Skalierungsgruppe mithilfe von PowerShell einrichten?

Siehe: https://msftstack.wordpress.com/2017/03/05/how-to-add-autoscale-to-an-azure-vm-scale-set/




## <a name="certificates"></a>Zertifikate

### <a name="how-do-you-securely-ship-a-certificate-into-the-vm--is-there-an-example-of-provisioning-a-scale-set-to-run-a-website-where-the-ssl-for-the-website-is-shipped-securely-from-a-certificate-configuration--the-common-certificate-rotation-operation-would-amount-to-a-configuration-update-operation"></a>Wie wird ein Zertifikat sicher auf den virtuellen Computer übertragen?  Gibt es ein Beispiel für die Bereitstellung einer Skalierungsgruppe, um eine Website auszuführen, wobei SSL für die Website sicher aus einer Zertifikatkonfiguration übertragen wird?  Der normale Zertifikatwechsel würde auf eine Konfigurationsaktualisierung hinauslaufen.

Wir unterstützen das Installieren von Kundenzertifikaten über den Schlüsseltresor direkt im Windows-Zertifikatspeicher.

Im Kontext von Skalierungsgruppen:

https://msdn.microsoft.com/library/mt589035.aspx

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

Damit werden Windows und Linux unterstützt.

### <a name="self-signed-certificate-example"></a>Beispiel für ein selbstsigniertes Zertifikat:

#### <a name="create-a-self-signed-cert-in-a-keyvault"></a>Erstellen eines selbstsignierten Zertifikats in einem Schlüsseltresor

Eine Möglichkeit, ein selbstsigniertes Zertifikat in einem Schlüsseltresor zu erstellen, ist, die Anweisungen in diesem Service Fabric-Artikel zu verwenden: https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/

Die PowerShell-Befehle:

```powershell
Import-Module "C:\Users\mikhegn\Downloads\Service-Fabric-master\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

Login-AzureRmAccount

Invoke-AddCertToKeyVault -SubscriptionId <Your SubID> -ResourceGroupName KeyVault -Location westus -VaultName MikhegnVault -CertificateName VMSSCert -Password VmssCert -CreateSelfSignedCertificate -DnsName vmss.mikhegn.azure.com -OutputPath c:\users\mikhegn\desktop\
```

Mit dem obigen Befehl erhalten Sie die Eingabe für die Resource Manager-Vorlage.

#### <a name="change-resource-manager-template"></a>Ändern der Resource Manager-Vorlage

Fügen Sie diese Eigenschaft „virtualMachineProfile“ als Teil der Skalierungsgruppenressource hinzu:

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
 

### <a name="is-there-a-way-to-specify-an-ssh-key-pair-that-i-want-to-use-for-ssh-authentication-with-a-linux-scale-set-from-a-resource-manager-template"></a>Gibt es eine Möglichkeit, ein SSH-Schlüsselpaar anzugeben, dass für die SSH-Authentifizierung bei einer Linux-Skalierungsgruppe über eine Resource Manager-Vorlage verwendet werden soll?  

Die REST-API für „osProfile“ sieht ungefähr wie bei einem normalen virtuellen Computer aus:
 
https://msdn.microsoft.com/library/azure/mt589035.aspx#linuxconfiguration
 
Nehmen Sie `osProfile` in die Vorlage auf, wie im folgenden Beispiel:

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
 
Dieser JSON-Block wird in der folgenden Schnellstartvorlage verwendet:
 
https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json
 
Betrachten Sie auch das Betriebssystemprofil in dieser Vorlage:
 
https://github.com/ExchMaster/gadgetron/blob/master/Gadgetron/Templates/grelayhost.json

### <a name="how-do-i-remove-deprecated-certificates"></a>Wie entferne ich veraltete Zertifikate? 

Sie müssen das alte Zertifikat aus der Liste der Tresorzertifikate entfernen, aber alle Zertifikate beibehalten, die auf dem Computer bleiben sollen. Dadurch wird das Zertifikat nicht von Ihren virtuellen Computern entfernt, aber das Zertifikat wird auch nicht zu neuen virtuellen Computern hinzugefügt, die in der Skalierungsgruppe erstellt werden. Um das Zertifikat von vorhandenen virtuellen Computern zu entfernen, müssen Sie eine benutzerdefinierte Skripterweiterung schreiben, die die Zertifikate manuell aus dem Zertifikatspeicher entfernt.
 
### <a name="how-do-i-take-an-existing-ssh-public-key-and-inject-it-into-the-scale-set-ssh-layer-during-provisioning--i-would-like-to-store-the-ssh-public-key-values-in-azure-key-vault-and-then-utilize-them-in-my-resource-manager-template"></a>Wie kann ich einen vorhandenen öffentlichen SSH-Schlüssel während der Bereitstellung in die SSH-Schicht der Skalierungsgruppe einfügen?  Ich möchte die Werte des öffentlichen SSH-Schlüssels in Azure Key Vault speichern und dann in der Resource Manager-Vorlage nutzen.

Wenn Sie den virtuellen Computern nur einen öffentlichen SSH-Schlüssel bereitstellen, besteht kein Grund, die öffentlichen Schlüssel in den Schlüsseltresor aufzunehmen, da öffentliche Schlüssel nicht geheim sind.
 
Sie können öffentliche SSH-Schlüssel bei der Erstellung einer Linux-VM im Nur-Text-Format bereitstellen.
Ein Beispiel finden Sie hier:

https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json
 
Dies gilt insbesondere in folgenden Fällen:

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
path | Ja | String | Gibt den Linux-Dateipfad an, in dem die SSH-Schlüssel oder -Zertifikate platziert werden sollten.
keyData | Ja | String | Gibt den Base64-codierten öffentlichen SSH-Schlüssel an.
 
### <a name="when-i-run-update-azurermvmss-after-more-than-one-certificate-from-the-same-keyvault-i-get-the-following-error"></a>Wenn ich „Update-AzureRmVmss“ nach mehr als einem Zertifikat aus dem gleichen Schlüsseltresor ausführe, wird die folgende Fehlermeldung angezeigt:
 
Update-AzureRmVmss: Die Liste der geheimen Schlüssel enthält wiederholte Instanzen von „/subscriptions/<Meine-Abonnement-ID>/resourceGroups/internal-rg-dev/providers/Microsoft.KeyVault/vaults/internal-keyvault-dev“, dies ist unzulässig. Warum kann ich nicht zwei Zertifikate aus dem gleichen Schlüsseltresor hinzufügen?
 
Dieses Verhalten kann auftreten, wenn Sie versuchen, den gleichen Tresor zweimal hinzuzufügen, statt eines neuen „vaultCertificate“ für den vorhandenen „sourceVault“. „Add-AzureRmVmssSecret“ funktioniert beim Hinzufügen von zusätzlichen geheimen Schlüsseln nicht ordnungsgemäß.
 
Wenn Sie weitere geheime Schlüssel aus dem gleichen Schlüsseltresor hinzufügen möchten, sollten Sie die Liste „$vmss.properties.osProfile.secrets[0].vaultCertificates“ aktualisieren.
 
Sie können die erwartete Eingabestruktur hier sehen: https://msdn.microsoft.com/library/azure/mt589035.aspx
 
Sie müssen den geheimen Schlüssel im Skalierungsgruppenobjekt finden, das im gleichen Schlüsseltresor enthalten ist. Dann müssen Sie den Zertifikatsverweis (die URL und den Namen den Speichers für geheime Schlüssel) der Liste hinzufügen, die dem Tresor zugeordnet ist.

Hinweis: Das Entfernen von Zertifikaten von virtuellen Computern über die Skalierungsgruppen-APIs wird derzeit nicht unterstützt.
 
Neue virtuelle Computer verfügen nicht über das alte Zertifikat, aber alte, auf denen das Zertifikat bereits bereitgestellt war, weisen weiterhin das alte Zertifikat auf.
 
### <a name="is-there-a-way-to-get-certificates-pushed-to-the-scale-set-without-providing-the-password-when-the-certificate-is-in-secretstore-currently"></a>Gibt es eine Möglichkeit, Zertifikate abzurufen, die per Push an die Skalierungsgruppe übertragen wurden, ohne das Kennwort anzugeben, wenn sich das Zertifikat derzeit im Speicher für geheime Schlüssel befindet?

Ein Hartcodierung von Kennwörtern in Skripts ist nicht erforderlich. Sie können sie dynamisch mit den jeweiligen Berechtigungen des Bereitstellungsskripts abrufen. Wenn Sie ein Skript verwenden, das ein Zertifikat aus dem Speicher für geheime Schlüssel in den Schlüsseltresor verschiebt, gibt der Befehl zum Abrufen des Zertifikats des Speichers für geheime Schlüssel außerdem das Kennwort der PFX-Datei aus.
 
### <a name="how-does-the-secrets-property-of-virtualmachineprofileosprofile-of-a-scale-set-work-why-do-you-need-sourcevault-when-you-have-to-specify-the-absolute-uri-to-a-certificate-with-certificateurl"></a>Wie funktioniert die Eigenschaft für geheime Schlüssel von „virtualMachineProfile.osProfile“ einer Skalierungsgruppe? Warum ist „sourceVault“ erforderlich, wenn der absolute URI zu einem Zertifikat mit „certificateUrl“ angeben werden muss? 

Ein Win RM-Zertifikatsverweis muss in der Eigenschaft für geheime Schlüssel des Betriebssystemprofils vorhanden sein. 

Der Quelltresor muss angegeben werden, damit ACL-Richtlinien erzwungen werden können, die in CSM vorhanden sind. Ohne die Angabe des Quelltresors könnten Benutzer über CRP auch ohne entsprechende Berechtigungen geheime Schlüssel in einem Schlüsseltresor bereitstellen bzw. darauf zugreifen. Die ACLs sind sogar für Ressourcen vorhanden, die nicht vorhanden sind.

Falls Sie eine falsche sourceVault-ID angegeben haben, aber eine gültige Schlüsseltresor-URL, würden wir einen Fehler melden, wenn Sie den Vorgang abrufen
 
### <a name="if-i-add-secrets-to-an-existing-scale-set-does-it-inject-them-in-existing-instances-or-only-new-ones"></a>Wenn ich geheime Schlüssel zu einer vorhandenen Skalierungsgruppe hinzufüge, werden sie auch in vorhandene Instanzen oder nur in neue eingefügt? 

Zertifikate werden zu allen virtuellen Computern hinzugefügt, auch zu bereits vorhandenen. Wenn die upgradePolicy-Eigenschaft der Skalierungsgruppe auf „Manuell“ festgelegt ist, wird das Zertifikat dem virtuellen Computer hinzugefügt, wenn Sie ein manuelles Update auf dem virtuellen Computer auszuführen.
 
### <a name="where-do-certificates-go-for-linux-vms"></a>Wohin kommen Zertifikate für virtuelle Linux-Computer?

Siehe: https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/
  
### <a name="how-do-you-add-a-new-vault-certificate-to-a-new-certificate-object"></a>Wie wird ein neues Tresorzertifikat zu einem neuen Zertifikatobjekt hinzugefügt?

Wenn Sie ein Tresorzertifikat zu einem vorhandenen geheimen Schlüssel hinzufügen möchten und dies das einzige geheime Objekt sein soll, können Sie vorgehen, wie im folgenden PowerShell-Beispiel gezeigt:
 
```powershell
$newVaultCertificate = New-AzureRmVmssVaultCertificateConfig -CertificateStore MY -CertificateUrl https://sansunallapps1.vault.azure.net:443/secrets/dg-private-enc/55fa0332edc44a84ad655298905f1809
 
$vmss.VirtualMachineProfile.OsProfile.Secrets[0].VaultCertificates.Add($newVaultCertificate)
 
Update-AzureRmVmss -VirtualMachineScaleSet $vmss -ResourceGroup $rg -Name $vmssName
```
 
### <a name="what-happens-to-certificates-if-you-reimage-a-vm"></a>Was geschieht mit Zertifikaten, wenn Sie für einen virtuellen Computer ein Reimaging durchführen?

Wenn Sie für einen virtuellen Computer ein Reimaging durchführen, werden die Zertifikate entfernt, da der Betriebssystemdatenträger vollständig gelöscht wird. 
 
### <a name="what-happens-if-you-delete-a-certificate-from-the-key-vault"></a>Was geschieht, wenn Sie ein Zertifikat aus dem Schlüsseltresor löschen?

Wenn der geheime Schlüssel im Schlüsseltresor gelöscht wird und Sie Ihre virtuellen Computer beenden, die Zuordnung aufheben und sie dann erneut starten, tritt ein Fehler auf. Die Ursache des Fehlers ist, dass CRP die geheimen Schlüssel aus dem Schlüsseltresor abrufen muss, es aber nicht kann. In diesem Szenario können Sie die Zertifikate aus dem Skalierungsgruppemodell löschen. 

In der CRP-Komponente werden keine geheimen Kundenschlüssel beibehalten. Wenn Sie alle virtuellen Computer in der Skalierungsgruppe beenden und die Zuordnung aufheben, wird der Cache gelöscht. In diesem Szenario werden die geheimen Schlüssel aus dem Schlüsseltresor abgerufen.

Dieses Problem tritt beim horizontalen Hochskalieren nicht auf, da es eine zwischengespeicherte Kopie des geheimen Schlüssels im Fabric gibt (zumindest im Mandantenmodell mit nur einem Fabric).
 
### <a name="why-do-we-have-to-specify-the-exact-location-for-the-certificate-url-as-referenced-here-per-httpsazuremicrosoftcomdocumentationarticlesservice-fabric-cluster-security"></a>Warum müssen wir den genauen Speicherort für die Zertifikat-URL angeben, wie hier erläutert: https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/, 
https://<name of the vault>.vault.azure.net:443/secrets/<exact location>
 
Gemäß der Dokumentation zum Schlüsseltresor sollte die get-secret-REST-API die neueste Version des geheimen Schlüssels zurückgeben, wenn keine Version angegeben ist:
 
Methode | URL
--- | ---
GET | https://mykeyvault.vault.azure.net/secrets/{Name des geheimen Schlüssels}/{Version des geheimen Schlüssels}?api-version={API-Version}

Ersetzen Sie {Name des geheimen Schlüssels} durch den Namen und {Version des geheimen Schlüssels} durch die Version des geheimen Schlüssels, den Sie abrufen möchten. Die Version des geheimen Schlüssels muss nicht angegeben werden. In diesem Fall wird die aktuelle Version abgerufen.
  
### <a name="why-does-certificate-version-have-to-be-specified-when-using-key-vault"></a>Warum muss die Zertifikatsversion angegeben werden, wenn der Schlüsseltresor verwendet wird?

Der Grund für diese Anforderung ist, dass die Benutzer genau wissen sollen, welches Zertifikat auf ihren virtuellen Computern bereitgestellt wird.

Wenn Sie einen virtuellen Computer erstellen und Ihren geheimen Schlüssel im Schlüsseltresor aktualisieren, wird das neue Zertifikat nicht auf Ihre virtuellen Computer heruntergeladen. Jedoch scheinen Ihre virtuellen Computer darauf zu verweisen, und neue virtuelle Computer erhalten den neuen geheimen Schlüssel. Um diese Probleme zu vermeiden, ist es erforderlich, dass Sie auf eine explizite Version des geheimen Schlüssels verweisen.

### <a name="my-team-works-with-several-certificates-that-are-distributed-to-us-as-cer-public-keys-what-is-the-recommended-approach-is-for-deployment-of-these-certs-to-a-scale-set"></a>Mein Team arbeitet mit mehreren Zertifikaten, die uns als öffentliche CER-Schlüssel zur Verfügung gestellt werden. Wie sieht die empfohlene Methode für die Bereitstellung dieser Zertifikate in einer Skalierungsgruppe aus?

Sie können mit „X509ContentType = Pfx“ eine PFX-Datei generieren, die nur CER-Dateien enthält. Laden Sie z.B. die CER-Datei als x509Certificate2-Objekt in C# oder PowerShell und rufen Sie diese Methode auf: https://msdn.microsoft.com/library/24ww6yzk(v=vs.110).aspx

### <a name="i-do-not-see-an-option-for-users-to-pass-in-certificates-as-base64-strings-that-most-other-resource-providers-provide"></a>Ich sehe keine Option für Benutzer, Zertifikate als Base64-Zeichenfolgen zu übergeben, die die meisten anderen Ressourcenanbieter bereitstellen.

Sie können die neueste URL mit Versionsangabe innerhalb einer Resource Manager-Vorlage extrahieren, um das Verhalten zu emulieren, das Sie beschreiben. Sie können die folgende JSON-Eigenschaft in die Resource Manager-Vorlage einschließen:

```json 
"certificateUrl": "[reference(resourceId(parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults/secrets', parameters('vaultName'), parameters('secretName')), '2015-06-01').secretUriWithVersion]"
```
 
### <a name="do-we-have-to-wrap-certs-in-json-objects-in-keyvaults"></a>Müssen wir Zertifikate in Schlüsseltresoren mit JSON-Objekten umschließen?

Dies ist eine Anforderung für Skalierungsgruppen/virtuelle Computer. Wir unterstützen zudem den Inhaltstyp „application/x-pkcs12“. Anweisungen finden Sie hier: http://www.rahulpnath.com/blog/pfx-certificate-in-azure-key-vault/
 
Wir unterstützen derzeit keine CER-Dateien. Sie müssen die CER-Dateien in PFX-Container exportieren.





## <a name="compliance"></a>Compliance

### <a name="are-scale-sets-pci-compliant"></a>Sind Skalierungsgruppen PCI-konform?

Skalierungsgruppen sind eine dünne API-Schicht über dem Computeressourcenanbieter. Dies ist alles Bestandteil des Bereichs „Computeplattform“ innerhalb der Azure-Dienststruktur.

Daher sind Skalierungsgruppen hinsichtlich der Konformität ein grundlegender Bestandteil der Azure-Computeplattform. Sie weisen die gleichen Einstellungen für Team, Tools, Prozesse, Bereitstellungsmethode, Sicherheitskontrollen, JIT, Überwachung, Warnungen usw. wie der Computeressourcenanbieter (CRP) selbst auf.  Skalierungsgruppen sind PCI-konform, da der Computeressourcenanbieter Teil des aktuellen PCI DSS-Nachweises ist:

Weitere Informationen finden Sie unter: [https://www.microsoft.com/TrustCenter/Compliance/PCI](https://www.microsoft.com/TrustCenter/Compliance/PCI).






## <a name="extensions"></a>Erweiterungen

### <a name="how-do-you-delete-a-scale-set-extension"></a>Wie löschen Sie eine Skalierungsgruppenerweiterung?

Dies ist ein Beispiel in PowerShell:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" 

$vmss=Remove-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "extensionName"

Update-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" -VirtualMacineScaleSet $vmss
```
 
„extensionName“ finden Sie in `$vmss`.
   
### <a name="is-there-a-scale-set-template-example-that-integrates-with-oms"></a>Gibt es ein Vorlagenbeispiel für eine Skalierungsgruppe, die in OMS integriert werden kann?

Sehen Sie sich das zweite Beispiel hier an:

https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/ServiceFabric
   
### <a name="extensions-seem-to-run-in-parallel-on-scale-sets-causing-my-custom-script-extension-to-fail-what-can-i-do-to-fix-this-behavior"></a>Erweiterungen werden scheinbar parallel in Skalierungsgruppen ausgeführt und führen dazu, dass meine benutzerdefinierte Skripterweiterung fehlschlägt. Wie kann ich dieses Verhalten korrigieren?

Siehe: https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/ 
 
 
### <a name="how-do-i-reset-the-password-for-scale-set-vms"></a>Wie setze ich das Kennwort für virtuelle Computer in Skalierungsgruppen zurück?

Verwenden Sie Zugriffserweiterungen für virtuelle Computer.

Dies ist ein Beispiel in PowerShell:

```powershell
$vmssName = "myvmss"
$vmssResourceGroup = "myvmssrg"
$publicConfig = @{"UserName" = "newuser"}
$privateConfig = @{"Password" = "********"}
 
$extName = "VMAccessAgent"
$publisher = "Microsoft.Compute"
$vmss = Get-AzureRmVmss -ResourceGroupName $vmssResourceGroup -VMScaleSetName $vmssName
$vmss = Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name $extName -Publisher $publisher -Setting $publicConfig -ProtectedSetting $privateConfig -Type $extName -TypeHandlerVersion "2.0" -AutoUpgradeMinorVersion $true
Update-AzureRmVmss -ResourceGroupName $vmssResourceGroup -Name $vmssName VirtualMachineScaleSet $vmss
```
 
 
### <a name="how-do-i-add-an-extension-to-all-vms-in-my-scale-set"></a>Wie füge ich allen virtuellen Computern in meiner Skalierungsgruppe eine Erweiterung hinzu?

- Wenn die Aktualisierungsrichtlinie auf „Automatisch“ festgelegt ist, werden durch die erneute Bereitstellung der Vorlage mit den neuen Erweiterungseigenschaften alle virtuellen Computer aktualisiert.
- Wenn die Aktualisierungsrichtlinie auf „Manuell“ festgelegt ist, müssen Sie die Erweiterung aktualisieren und dann „manualUpdate“ für alle Instanzen ausführen.
  
### <a name="if-the-extensions-associated-with-an-existing-scale-set-are-updated-would-they-affect-already-existing-vms-that-is-would-the-vms-show-up-as-not-matching-the-scale-set-model-or-would-they-be-ignored-when-an-existing-machine-is-service-healed--reimaged--etc-would-the-scripts-that-are-currently-configured-on-the-scale-set-be-executed-or-would-the-ones-that-were-configured-when-the-machine-was-first-created-be-used"></a>Hätte es Auswirkungen auf bereits vorhandene virtuelle Computer, wenn die einer vorhandenen Skalierungsgruppe zugeordneten Erweiterungen aktualisiert werden? (Würden die virtuellen Computer also im Skalierungsgruppenmodell als nicht übereinstimmend angezeigt?) Oder werden sie ignoriert? Wenn ein vorhandener Computer vom Dienst repariert wird oder ein Reimaging durchgeführt wird, werden dann die Skripts ausgeführt, die gegenwärtig für die Skalierungsgruppe konfiguriert sind, oder werden die Skripts verwendet, die bei der Erstellung des Computers konfiguriert wurden?

- Wenn die Erweiterungsdefinition im Skalierungsgruppenmodell aktualisiert wird, werden auch die virtuellen Computer aktualisiert, sofern „upgradePolicy“ auf „Automatisch“ festgelegt wurde. Und sie werden als nicht mit dem Modell übereinstimmend markiert, wenn „upgradePolicy“ auf „Manuell“ festgelegt ist. 

- Wenn ein vorhandener virtueller Computer vom Dienst repariert wird, wirkt dies wie ein Neustart, und die Erweiterungen werden nicht erneut ausgeführt. Wenn ein Reimaging durchgeführt wird, entspricht dies dem Ersetzen des Betriebssystemlaufwerks durch das Quellimage, und alle Spezialisierungen aus dem aktuellen Modell, wie Erweiterungen, werden ausgeführt.
 
### <a name="how-do-i-get-a-scale-set-to-join-an-ad-domain"></a>Wie kann ich eine Skalierungsgruppe in eine AD-Domäne aufnehmen?

Sie könnten eine Erweiterung wie die folgende mit „JsonADDomainExtension“ definieren:
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
 
### <a name="my-scale-set-extension-is-trying-to-install-something-that-requires-a-reboot-for-instance-commandtoexecute-powershellexe--executionpolicy-unrestricted-install-windowsfeature-name-fs-resource-manager-includemanagementtools"></a>Meine Skalierungsgruppenerweiterung versucht, etwas zu installieren, das einen Neustart erfordert, z.B.: commandToExecute: powershell.exe -ExecutionPolicy Unrestricted Install-WindowsFeature –Name FS-Resource-Manager –IncludeManagementTools

Sie könnten die DSC-Erweiterung verwenden. Wenn das Betriebssystem 2012 R2 ist, führt Azure das WMF 5.0-Setup aus, startet neu und setzt die Konfiguration fort. 
 
### <a name="how-can-i-enable-antimalware-on-my-scale-set"></a>Wie kann ich Antischadsoftware für meine Skalierungsgruppe aktivieren?

Hier ist ein PowerShell-Beispiel:

```powershell
$rgname = 'autolap'
$vmssname = 'autolapbr'
$location = 'eastus'
 
# retrieve the most recent version number of the extension
$allVersions= (Get-AzureRmVMExtensionImage -Location $location -PublisherName "Microsoft.Azure.Security" -Type "IaaSAntimalware").Version
$versionString = $allVersions[($allVersions.count)-1].Split(".")[0] + "." + $allVersions[($allVersions.count)-1].Split(".")[1]
 
$VMSS = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname
echo $VMSS
Add-AzureRmVmssExtension -VirtualMachineScaleSet $VMSS -Name "IaaSAntimalware" -Publisher "Microsoft.Azure.Security" -Type "IaaSAntimalware" -TypeHandlerVersion $versionString
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $VMSS 
```

### <a name="i-need-to-execute-a-custom-script-hosted-on-a-private-storage-account-i-have-no-problems-when-the-storage-is-public-but-when-i-try-to-use-a-shared-access-signaturesas-it-fails-with-the-error-missing-mandatory-parameters-for-valid-shared-access-signature-i-know-that-linksas-works-fine-from-my-local-browser"></a>Ich muss ein benutzerdefiniertes Skript ausführen, das in einem privaten Speicherkonto gehostet wird. Ich habe keine Probleme, wenn der Speicher öffentlich ist, aber wenn ich versuche, eine SAS (Shared Access Signature) zu verwenden, tritt ein Fehler auf. Die Fehlermeldung lautet: „Erforderliche Parameter für gültige SAS (Shared Access Signature) fehlen“. Ich weiß, dass dieser Link und SAS über meinen lokalen Browser funktionieren.

Damit dieses Szenario funktioniert, müssen Sie geschützte Einstellungen für den Speicherkontoschlüssel und den Namen einrichten. Siehe: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/#template-example-for-a-windows-vm-with-protected-settings







## <a name="networking"></a>Netzwerk
 
### <a name="how-do-i-do-vip-swap-for-scale-sets-in-the-same-subscription-and-same-region"></a>Wie wird ein VIP-Swap für Skalierungsgruppen im selben Abonnement und derselben Region ausgeführt?

Siehe: https://msftstack.wordpress.com/2017/02/24/vip-swap-blue-green-deployment-in-azure-resource-manager/ 
 
  
### <a name="what-is-the-resourceguid-property-on-a-nic-for-its-not-documented-here"></a>Wozu dient die resourceGuid-Eigenschaft einer Netzwerkkarte? Dies wird hier nicht dokumentiert.

Es ist eine eindeutige ID. Untere Schichten werden diese ID irgendwann in der Zukunft protokollieren. 
 
### <a name="how-do-i-specify-a-range-of-private-ip-addresses-for-static-private-ip-address-allocation"></a>Wie gebe ich einen Bereich von privaten IP-Adressen für die statische private IP-Adresszuordnung an?

IP-Adressen werden aus einem Subnetz ausgewählt, das Sie angeben. 

Die Zuordnungsmethode für IP-Adressen von Skalierungsgruppen ist immer „Dynamisch“. Dies bedeutet jedoch nicht, dass sich diese IP-Adressen ändern können. Es bedeutet lediglich, dass Sie keine IP-Adressen in PUT-Anforderungen angeben müssen. Anders ausgedrückt: Sie geben den statischen Satz mit dem Subnetz an. 
    
### <a name="how-do-i-deploy-a-scale-set-into-an-existing-vnet"></a>Wie stelle ich eine Skalierungsgruppe in einem vorhandenen VNET bereit? 

Siehe: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-existing-vnet 




## <a name="scale"></a>Skalieren

### <a name="why-would-you-ever-create-a-scale-set-with-fewer-than-2-vms"></a>Welche Gründe gibt es, eine Skalierungsgruppe mit weniger als zwei virtuellen Computern zu erstellen?

Ein Grund wäre, die elastischen Eigenschaften einer Skalierungsgruppe zu nutzen. Beispielsweise könnten Sie eine Skalierungsgruppe ohne virtuelle Computer bereitstellen, um Ihre Infrastruktur zu definieren, ohne die laufenden Kosten für virtuelle Computer zu bezahlen. Wenn Sie dann für die Bereitstellung von virtuellen Computern bereit sind, können Sie dazu die „Kapazität“ der Skalierungsgruppe auf die Anzahl der Produktionsinstanzen erhöhen.

Ein weiterer Grund dafür wäre, dass Sie die Skalierungsgruppe auf eine Weise nutzen, bei der die Verfügbarkeit nicht die gleiche Bedeutung hat wie bei der Verwendung einer Verfügbarkeitsgruppe mit diskreten virtuellen Computern. Skalierungsgruppen bieten eine Möglichkeit, mit undifferenzierten Computeeinheiten zu arbeiten, die austauschbar sind. Diese Einheitlichkeit ist ein wichtiges Unterscheidungsmerkmal von Skalierungsgruppen im Vergleich mit Verfügbarkeitsgruppen. Für viele zustandslose Workloads sind die einzelnen Einheiten nicht von Bedeutung. Sie können auf eine Computeeinheit herunterskaliert werden, wenn die Workload zurückgeht, und wieder auf viele hochskaliert werden, wenn sich die Workload erhöht.

### <a name="how-do-you-change-the-number-of-vms-in-a-scale-set"></a>Wie ändern Sie die Anzahl der virtuellen Computer in einer Skalierungsgruppe?

Siehe: https://msftstack.wordpress.com/2016/05/13/change-the-instance-count-of-an-azure-vm-scale-set/

### <a name="how-can-you-define-custom-alerts-for-when-certain-thresholds-are-reached"></a>Wie können Sie benutzerdefinierte Warnungen für das Erreichen bestimmter Schwellenwerte definieren?

Bei der Behandlung von Warnungen gibt es eine gewisse Flexibilität. Sie können z.B. benutzerdefinierte Webhooks definieren, wie in diesem Beispiel aus einer Resource Manager-Vorlage veranschaulicht:
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

In diesem Beispiel wird eine Warnung an Pagerduty ausgegeben, wenn ein Schwellenwert erreicht wird.



## <a name="troubleshooting"></a>Problembehandlung

### <a name="how-do-i-enable-boot-diagnostics"></a>Wie aktiviere ich die Startdiagnose?

Erstellen Sie ein Speicherkonto, fügen Sie den folgenden JSON-Block in „virtualMachineProfile“ der Skalierungsgruppe ein, und aktualisieren Sie die Skalierungsgruppe:
```json
      "diagnosticsProfile": {
        "bootDiagnostics": {
          "enabled": true,
          "storageUri": "http://yourstorageaccount.blob.core.windows.net"
        }
      }
```

Beim Erstellen eines neuen virtuellen Computers zeigt dann „InstanceView“ des virtuellen Computers die Details für den Screenshot usw. an. Beispiel:
 
```json
"bootDiagnostics": {
    "consoleScreenshotBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.screenshot.bmp",
    "serialConsoleLogBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.serialconsole.log"
  }
```

 

## <a name="updates"></a>Aktualisierungen

### <a name="how-to-i-update-my-scale-set-to-a-new-image-and-manage-patching"></a>Wie kann ich meine Skalierungsgruppe auf ein neues Image aktualisieren und das Patchen verwalten?

Siehe: https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set

### <a name="can-you-use-the-reimage-operation-to-reset-a-vm-without-changing-the-image-that-is-reset-a-vm-to-factory-settings-rather-than-to-a-new-image"></a>Kann ein virtueller Computer durch Reimaging zurückgesetzt werden, ohne das Image zu ändern? (Kann er also auf die Werkseinstellungen zurückgesetzt werden, statt auf ein neues Image?)

Ja. Siehe: https://docs.microsoft.com/rest/api/virtualmachinescalesets/manage-all-vms-in-a-set

Wenn die Skalierungsgruppe jedoch auf ein Plattformimage mit version = "latest" verweist, kann Ihr virtueller Computer auf ein höheres Betriebssystemimage aktualisiert werden, wenn Sie das Reimaging durchführen.







## <a name="vm-properties"></a>Eigenschaften virtueller Computer

### <a name="how-do-i-get-property-information-for-each-vm-without-having-to-make-multiple-calls-for-example-getting-the-fault-domain-for-each-vm-in-my-100-scale-set"></a>Wie erhalte ich Informationen zu den Eigenschaften für jeden virtuellen Computer, ohne mehrere Aufrufe auszuführen? Beispiel: Abrufen der Fehlerdomäne für jeden virtuellen Computer in meiner Skalierungsgruppe mit 100 virtuellen Computern?

Sie können „ListVMInstanceViews“ aufrufen, indem Sie einen `GET`-REST-API-Vorgang über den folgenden Ressourcen-URI ausführen:

`/subscriptions/<subscription_id>/resourceGroups/<resource_group_name>/providers/Microsoft.Compute/virtualMachineScaleSets/<scaleset_name>/virtualMachines?$expand=instanceView&$select=instanceView`

### <a name="are-there-ways-to-pass-different-extension-arguments-to-different-vms-in-a-scale-set"></a>Gibt es Methoden zum Übergeben von unterschiedlichen Erweiterungsargumenten an verschiedene virtuelle Computer in einer Skalierungsgruppe?

Nein, aber Erweiterungen können basierend auf eindeutigen Eigenschaften des virtuellen Computers, auf dem sie ausgeführt werden (z.B. Computername), genutzt werden. Darüber hinaus können Erweiterungen Instanzmetadaten von „http://169.254.169.254“ abfragen, um weitere Informationen zu erhalten.

### <a name="why-are-there-gaps-between-my-scale-set-vm-machine-names-and-vm-ids-for-example-0-1-3"></a>Warum gibt es Lücken zwischen den Computernamen in der VM-Skalierungsgruppe und den IDs virtueller Computer? Beispiel: 0, 1, 3...

Es gibt Lücken, da die Eigenschaft für die Überprovisionierung der Skalierungsgruppe auf den Standardwert „true“ festgelegt ist. Bei einer Überprovisionierung werden mehr virtuelle Computer als angefordert erstellt, und die zusätzlichen virtuellen Computer werden anschließend gelöscht. Sie erreichen dadurch eine erhöhte Zuverlässigkeit der Bereitstellung auf Kosten von fortlaufenden Namen und zusammenhängenden NAT-Regeln. Sie können diese Eigenschaft auf „false“ festlegen, und bei kleinen Skalierungsgruppen ergibt sich kein großer Unterschied bei der Zuverlässigkeit der Bereitstellung.

### <a name="what-is-the-difference-between-deleting-a-vm-in-a-scale-set-vs-deallocating-the-vm-when-should-i-choose-one-over-the-other"></a>Was ist der Unterschied zwischen dem Löschen eines virtuellen Computers in einer Skalierungsgruppe und dem Aufheben der Zuordnung des virtuellen Computers? Wann sollte ich was wählen?

Der Hauptunterschied besteht darin, dass beim Aufheben der Zuordnung die VHDs nicht gelöscht werden, daher sind keine Speicherkosten mit dem Beenden und Aufheben der Zuordnung verbunden. Mögliche Gründe für Ihre Entscheidung:

- Sie möchten nicht weiter für Compute zahlen, jedoch den Datenträgerzustand der virtuellen Computer beibehalten.
- Sie möchten eine Gruppe von virtuellen Computern schneller starten, als Sie eine Skalierungsgruppe horizontal hochskalieren können.
  - Im Zusammenhang mit diesem Szenario: Sie haben ein eigenes Modul für die automatische Skalierung erstellt und wünschen sich eine schnellere End-to-End-Skalierung.
  - Sie verfügen über eine Skalierungsgruppe, die ungleichmäßig über FD/UDs verteilt ist (da virtuelle Computer selektiv gelöscht wurden oder da virtuelle Computer nach einer Überprovisionierung gelöscht wurden). Durch Beenden und Aufheben der Zuordnung gefolgt vom Starten in der Skalierungsgruppe werden die virtuellen Computer gleichmäßig über FD/UDs verteilt.








 
   

