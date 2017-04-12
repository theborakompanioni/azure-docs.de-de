---
title: "Verschlüsseln von Datenträgern auf einem virtuellen Linux-Computer mithilfe von Azure CLI 1.0 | Microsoft-Dokumentation"
description: "Verschlüsseln von Datenträgern auf einem virtuellen Linux-Computer unter Verwendung von Azure CLI 1.0 und des Resource Manager-Bereitstellungsmodells"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/06/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 5108df1ef407132de4c685d35f1c453d30d1aa96
ms.lasthandoff: 04/03/2017


---
# <a name="encrypt-disks-on-a-linux-vm-using-the-azure-cli-10"></a>Verschlüsseln von Datenträgern auf einem virtuellen Linux-Computer mithilfe von Azure CLI 1.0
Zur Verbesserung der Sicherheit und Compliance von virtuellen Computern können virtuelle Datenträger in Azure im Ruhezustand verschlüsselt werden. Die Verschlüsselung der Datenträger basiert auf kryptografischen Schlüsseln, die in Azure Key Vault gesichert werden. Diese kryptografischen Schlüssel werden von Ihnen kontrolliert, und Sie können deren Verwendung überwachen. In diesem Artikel erfahren Sie, wie Sie virtuelle Datenträger auf einem virtuellen Linux-Computer unter Verwendung von Azure CLI 1.0 und des Resource Manager-Bereitstellungsmodells verschlüsseln.

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe
Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- [Azure-CLI 1.0](#quick-commands): Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell (in diesem Artikel)
- [Azure CLI 2.0:](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell

## <a name="quick-commands"></a>Schnellbefehle
Der folgende Abschnitt enthält Informationen zu den grundlegenden Befehlen für die Verschlüsselung virtueller Datenträger auf Ihrem virtuellen Computer. Ausführlichere Informationen und Kontext für die einzelnen Schritte finden Sie im übrigen Dokument ([ab hier](#overview-of-disk-encryption)).

Installieren Sie die [neueste Version von Azure CLI 1.0](../../xplat-cli-install.md), und melden Sie sich wie folgt im Resource Manager-Modus an:

```azurecli
azure config mode arm
```

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Als Beispielparameternamen werden `myResourceGroup`, `myKeyVault` und `myVM` verwendet.

Aktivieren Sie zunächst in Ihrem Azure-Abonnement den Azure Key Vault-Anbieter, und erstellen Sie eine Ressourcengruppe. Im folgenden Beispiel wird eine Ressourcengruppe namens `myResourceGroup` am Standort `WestUS` erstellt:

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Erstellen Sie eine Azure Key Vault-Instanz. Im folgenden Beispiel wird eine Key Vault-Instanz namens `myKeyVault` erstellt:

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Erstellen Sie in Key Vault einen kryptografischen Schlüssel, und aktivieren Sie ihn für die Datenträgerverschlüsselung. Im folgenden Beispiel wird ein Schlüssel namens `myKey` erstellt:

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

Erstellen Sie einen Endpunkt, und verwenden Sie dabei Azure Active Directory für die Behandlung der Authentifizierung und den Austausch von kryptografischen Schlüsseln aus Key Vault. `--home-page` und `--identifier-uris` müssen keine echten routingfähigen Adressen sein. Es empfiehlt sich, anstelle von Kennwörtern geheime Clientschlüssel zu verwenden, um ein Höchstmaß an Sicherheit zu erreichen. Die Azure-Befehlszeilenschnittstelle kann derzeit keine geheimen Clientschlüssel generieren. Geheime Clientschlüssel können nur im Azure-Portal generiert werden. Im folgenden Beispiel wird ein Azure Active Directory-Endpunkt namens `myAADApp` erstellt und das Kennwort `myPassword` verwendet. Gehen Sie zum Angeben eines eigenen Kennworts wie folgt vor:

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Beachten Sie in der Ausgabe des vorherigen Befehls die `applicationId`. Diese Anwendungs-ID wird in den folgenden Schritten verwendet:

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

Fügen Sie einem vorhandenen virtuellen Computer einen Datenträger hinzu. Im folgenden Beispiel wird einem vorhandenen virtuellen Computer namens `myVM` ein Datenträger hinzugefügt:

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Überprüfen Sie die Details für Ihre Key Vault-Instanz und den erstellten Schlüssel. Die Key Vault-ID, der URI und die Schlüssel-URL werden im letzten Schritt benötigt. Im folgenden Beispiel werden die Details für eine Key Vault-Instanz namens `myKeyVault` und einen Schlüssel namens `myKey` überprüft:

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Verschlüsseln Sie Ihre Datenträger wie folgt, und geben Sie dabei überall Ihre eigenen Parameternamen ein:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Die Azure-Befehlszeilenschnittstelle stellt während des Verschlüsselungsprozesses keine ausführlichen Fehler bereit. Weitere Informationen zur Problembehandlung finden Sie in der Datei `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`. Da der vorherige Befehl zahlreiche Variablen enthält und Sie unter Umständen nicht genau erfahren, warum der Prozess nicht erfolgreich war, finden Sie im Anschluss ein Beispiel für einen vollständigen Befehl:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Prüfen Sie abschließend erneut den Verschlüsselungsstatus, um sich zu vergewissern, dass Ihre virtuellen Datenträger nun verschlüsselt sind. Im folgenden Beispiel wird der Status eines virtuellen Computers namens `myVM` in der Ressourcengruppe `myResourceGroup` überprüft:

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a>Übersicht über die Datenträgerverschlüsselung
Virtuelle Datenträger auf virtuellen Linux-Computern werden im Ruhezustand mithilfe von [dm-crypt](https://wikipedia.org/wiki/Dm-crypt) verschlüsselt. Für die Verschlüsselung virtueller Datenträger in Azure fallen keine Gebühren an. Kryptografische Schlüssel werden in Azure Key Vault mit Softwareschutz gespeichert. Alternativ können Sie Schlüssel aber auch in Hardwaresicherheitsmodulen (HSMs) mit FIPS 140-2 Level 2-Zertifizierung importieren oder generieren. Diese kryptografischen Schlüssel werden allein von Ihnen kontrolliert, und Sie können deren Verwendung überwachen. Die kryptografischen Schlüssel dienen zum Verschlüsseln und Entschlüsseln virtueller Datenträger, die an Ihren virtuellen Computer angefügt sind. Über einen Azure Active Directory-Endpunkt werden die kryptografischen Schlüssel beim Ein- und Ausschalten virtueller Computer sicher ausgegeben.

Gehen Sie zum Verschlüsseln eines virtuellen Computers wie folgt vor:

1. Erstellen Sie in einer Azure Key Vault-Instanz einen kryptografischen Schlüssel.
2. Konfigurieren Sie den kryptografischen Schlüssel so, dass er zum Verschlüsseln von Datenträgern verwendet werden kann.
3. Erstellen Sie einen Endpunkt mit Azure Active Directory und den entsprechenden Berechtigungen, um den kryptografischen Schlüssel aus der Azure Key Vault-Instanz zu lesen.
4. Führen Sie den Befehl zum Verschlüsseln Ihrer virtuellen Datenträger aus. Geben Sie dabei den Azure Active Directory-Endpunkt und den zu verwendenden kryptografischen Schlüssel an.
5. Der Azure Active Directory-Endpunkt fordert den erforderlichen kryptografischen Schlüssel von Azure Key Vault an.
6. Die virtuellen Datenträger werden mithilfe des angegebenen kryptografischen Schlüssels verschlüsselt.

## <a name="supporting-services-and-encryption-process"></a>Unterstützende Dienste und Verschlüsselungsprozess
Für die Datenträgerverschlüsselung werden folgende zusätzliche Komponenten benötigt:

* **Azure Key Vault:** Schützt die für die Datenträgerverschlüsselung/-entschlüsselung verwendeten kryptografischen und geheimen Schlüssel. 
  * Sie können eine ggf. bereits vorhandene Azure Key Vault-Instanz verwenden. Für die Datenträgerverschlüsselung wird keine dedizierte Key Vault-Instanz benötigt.
  * Zur Trennung administrativer Grenzen und der Schlüsselsichtbarkeit können Sie eine dedizierte Key Vault-Instanz erstellen.
* **Azure Active Directory:** Wickelt den sicheren Austausch der erforderlichen kryptografischen Schlüssel und die Authentifizierung für angeforderte Aktionen ab. 
  * In der Regel können Sie Ihre Anwendung in einer bereits vorhandenen Instanz von Azure Active Directory platzieren. 
  * Die Anwendung ist eher eine Art Endpunkt für den Key Vault- und den Virtual Machine-Dienst und dient zum Anfordern und Beziehen der entsprechenden ausgegebenen kryptografischen Schlüssel. Sie entwickeln im eigentlichen Sinne keine Anwendung, die in Azure Active Directory integriert wird.

## <a name="requirements-and-limitations"></a>Voraussetzungen und Einschränkungen
Unterstützte Szenarien und Voraussetzungen für die Datenträgerverschlüsselung:

* Folgende Linux-Server-SKUs: Ubuntu, CentOS, SUSE und SUSE Linux Enterprise Server (SLES) sowie Red Hat Enterprise Linux
* Alle Ressourcen (wie Key Vault, Speicherkonto und virtueller Computer) müssen sich in der gleichen Azure-Region und im gleichen Abonnement befinden.
* Virtuelle Computer der standardmäßigen A-, D-, DS-, G- und GS-Serie

Die Datenträgerverschlüsselung wird derzeit in folgenden Szenarien nicht unterstützt:

* Bei virtuellen Computern des Basic-Tarifs
* Bei virtuellen Computern, die mit dem klassischen Bereitstellungsmodell erstellt wurden
* Bei Deaktivierung der Betriebssystem-Datenträgerverschlüsselung auf virtuellen Linux-Computern
* Bei Aktualisierung der kryptografischen Schlüssel auf einem bereits verschlüsselten virtuellen Linux-Computer

## <a name="create-the-azure-key-vault-and-keys"></a>Erstellen der Azure Key Vault-Instanz und der Schlüssel
Installieren Sie die [neueste Version von Azure CLI 1.0](../../xplat-cli-install.md), und melden Sie sich wie folgt im Resource Manager-Modus an, um die restlichen Schritte in diesem Leitfaden ausführen zu können:

```azurecli
azure config mode arm
```

Ersetzen Sie bei den Befehlsbeispielen alle Beispielparameter durch Ihre eigenen Namen, Orte und Schlüsselwerte. In den folgenden Beispielen wird die Konvention `myResourceGroup`, `myKeyVault`, `myAADApp` usw. verwendet.

Erstellen Sie zum Speichern Ihrer kryptografischen Schlüssel zunächst eine Azure Key Vault-Instanz. In Azure Key Vault können Schlüssel, geheime Schlüssel und Kennwörter gespeichert werden, um eine sichere Implementierung in Anwendungen und Diensten zu ermöglichen. Bei der Verschlüsselung virtueller Datenträger dient Key Vault zum Speichern eines kryptografischen Schlüssels, der zum Verschlüsseln oder Entschlüsseln der virtuellen Datenträger verwendet wird. 

Aktivieren Sie in Ihrem Azure-Abonnement den Azure Key Vault-Anbieter, und erstellen Sie anschließend eine Ressourcengruppe. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `WestUS`:

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Die Azure Key Vault-Instanz mit den kryptografischen Schlüsseln und die dazugehörigen Computeressourcen wie Speicher und der eigentliche virtuelle Computer müssen sich in der gleichen Region befinden. Im folgenden Beispiel wird eine Azure Key Vault-Instanz namens `myKeyVault` erstellt:

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Kryptografische Schlüssel können mit Softwareschutz oder mit HSM-Schutz (Hardwaresicherheitsmodul) gespeichert werden. Für die Verwendung eines HSMs wird eine Key Vault-Premiuminstanz benötigt. Die Erstellung einer Key Vault-Premiuminstanz ist im Gegensatz zur Verwendung einer Key Vault-Standardinstanz, bei der Schlüssel mit Softwareschutz gespeichert werden, mit zusätzlichen Kosten verbunden. Wenn Sie eine Key Vault-Premiuminstanz erstellen möchten, fügen Sie dem Befehl aus dem vorherigen Schritt `--sku Premium` hinzu. Im folgenden Beispiel werden softwaregeschützte Schlüssel verwendet, da wir eine Key Vault-Standardinstanz erstellt haben. 

Bei beiden Schutzmodellen muss der Azure-Plattform Zugriff gewährt werden, um beim Start des virtuellen Computers die kryptografischen Schlüssel anfordern und die virtuellen Datenträger entschlüsseln zu können. Erstellen Sie in Ihrer Key Vault-Instanz einen Verschlüsselungsschlüssel, und aktivieren Sie ihn für die Verschlüsselung virtueller Datenträger. Im folgenden Beispiel wird ein Schlüssel namens `myKey` erstellt und anschließend für die Datenträgerverschlüsselung aktiviert:

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-the-azure-active-directory-application"></a>Erstellen der Azure Active Directory-Anwendung
Wenn virtuelle Datenträger verschlüsselt oder entschlüsselt werden, verwenden Sie einen Endpunkt, der die Authentifizierung und den Austausch kryptografischer Schlüssel aus Key Vault abwickelt. Dieser Endpunkt (eine Azure Active Directory-Anwendung) ermöglicht es der Azure-Plattform, im Auftrag des virtuellen Computers die entsprechenden kryptografischen Schlüssel anzufordern. In Ihrem Abonnement steht zwar eine Azure Active Directory-Standardinstanz zur Verfügung, viele Organisationen verwenden jedoch dedizierte Azure Active Directory-Verzeichnisse.

Da Sie keine vollständige Azure Active Directory-Anwendung erstellen, muss es sich bei den Parametern `--home-page` und `--identifier-uris` im folgenden Beispiel nicht um echte routingfähige Adressen handeln. Im folgenden Beispiel wird außerdem ein kennwortbasierter geheimer Schlüssel angegeben, anstatt Schlüssel über das Azure-Portal zu generieren. Derzeit können über die Azure-Befehlszeilenschnittstelle keine Schlüssel generiert werden. 

Erstellen Sie Ihre Azure Active Directory-Anwendung. Im folgenden Beispiel wird eine Anwendung namens `myAADApp` erstellt und das Kennwort `myPassword` verwendet. Gehen Sie zum Angeben eines eigenen Kennworts wie folgt vor:

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Notieren Sie sich die `applicationId`, die in der Ausgabe des vorherigen Befehls zurückgegeben wird. Diese Anwendungs-ID wird in einigen der verbleibenden Schritte verwendet. Erstellen Sie anschließend einen Dienstprinzipalnamen (Service Principal Name, SPN), damit innerhalb Ihrer Umgebung auf die Anwendung zugegriffen werden kann. Zur erfolgreichen Ver- und Entschlüsselung virtueller Datenträger müssen Berechtigungen für den in Key Vault gespeicherten kryptografischen Schlüssel festgelegt werden, damit die Azure Active Directory-Anwendung die Schlüssel lesen kann. 

Erstellen Sie den SPN wie folgt, und legen Sie die entsprechenden Berechtigungen fest:

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a>Hinzufügen eines virtuellen Datenträgers und Überprüfen des Verschlüsselungsstatus
Im diesem Abschnitt fügen wir einem vorhandenen virtuellen Computer einen Datenträger hinzu, um die Verschlüsselung virtueller Datenträger in der Praxis anzuwenden. Fügen Sie einem vorhandenen virtuellen Computer wie folgt einen 5-GB-Datenträger hinzu:

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Die virtuellen Datenträger sind aktuell nicht verschlüsselt. Überprüfen Sie wie folgt den aktuellen Verschlüsselungsstatus des virtuellen Computers:

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a>Verschlüsseln virtueller Datenträger
Zur Verschlüsselung der virtuellen Datenträger müssen alle vorherigen Komponenten miteinander kombiniert werden:

1. Geben Sie die Azure Active Directory-Anwendung und das Kennwort an.
2. Geben Sie die Key Vault-Instanz zum Speichern der Metadaten Ihrer verschlüsselten Datenträger an.
3. Geben Sie die kryptografischen Schlüssel für die eigentliche Ver- und Entschlüsselung an.
4. Geben Sie an, ob Sie den Betriebssystem-Datenträger, die allgemeinen Datenträger oder alles verschlüsseln möchten.

Sehen Sie sich die Details für Ihre Azure Key Vault-Instanz und den erstellten Schlüssel an, da Sie im letzten Schritt die Key Vault-ID, den URI und die Schlüssel-URL benötigen:

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Verschlüsseln Sie Ihre virtuellen Datenträger wie folgt unter Verwendung der Ausgabe der Befehle `azure keyvault show` und `azure keyvault key show`:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Da der vorherige Befehl zahlreiche Variablen enthält, finden Sie im Anschluss ein Beispiel für einen vollständigen Befehl:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Die Azure-Befehlszeilenschnittstelle stellt während des Verschlüsselungsprozesses keine ausführlichen Fehler bereit. Weitere Informationen zur Problembehandlung finden Sie auf dem zu verschlüsselnden virtuellen Computer in der Datei `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`.

Prüfen Sie abschließend erneut den Verschlüsselungsstatus, um sich zu vergewissern, dass Ihre virtuellen Datenträger nun verschlüsselt sind:

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a>Hinzufügen zusätzlicher Datenträger
Nachdem Sie Ihre Datenträger verschlüsselt haben, können Sie dem virtuellen Computer später weitere virtuelle Datenträger hinzufügen und diese ebenfalls verschlüsseln. Erhöhen Sie beim Ausführen des Befehls `azure vm enable-disk-encryption` die Sequenzversion mithilfe des `--sequence-version`-Parameters. Mit diesem Sequenzversionsparameter können Sie wiederholte Vorgänge auf dem gleichen virtuellen Computer ausführen.

So können Sie Ihrem virtuellen Computer beispielsweise wie folgt einen zweiten virtuellen Datenträger hinzufügen:

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Führen Sie den Befehl erneut aus, um die virtuellen Datenträger zu verschlüsseln. Fügen Sie diesmal allerdings den `--sequence-version`-Parameter hinzu, und erhöhen Sie den Wert aus der ersten Ausführung wie folgt:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
  --sequence-version 2
```


## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Verwalten von Azure Key Vault (einschließlich Informationen zum Löschen kryptografischer Schlüssel und Vault-Instanzen) finden Sie unter [Verwalten von Schlüsseltresor mit CLI](../../key-vault/key-vault-manage-with-cli.md).
* Weitere Informationen zur Datenträgerverschlüsselung (etwa zum Vorbereiten des Uploads eines verschlüsselten benutzerdefinierten virtuellen Computers in Azure) finden Sie unter [Azure-Datenträgerverschlüsselung für virtuelle Windows- und Linux-IaaS-Computer](../../security/azure-security-disk-encryption.md).


