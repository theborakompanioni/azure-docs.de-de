---
title: Sichern eines Webservers mit SSL-Zertifikaten in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den NGINX-Webserver mit SSL-Zertifikaten auf einem virtuellen Linux-Computer in Azure sichern.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/17/2017
ms.author: iainfou
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 181be35aeb61020db3abaeba22aa882848923c31
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---

# <a name="secure-a-web-server-with-ssl-certificates-on-a-linux-virtual-machine-in-azure"></a>Sichern eines Webservers mit SSL-Zertifikaten auf einem virtuellen Linux-Computer in Azure
Zum Sichern von Webservern kann ein SSL-Zertifikat (Secure Sockets Layer) zum Verschlüsseln des Webdatenverkehrs verwendet werden. Diese SSL-Zertifikate können in Azure Key Vault gespeichert werden. Sie ermöglichen sichere Bereitstellungen von Zertifikaten auf virtuellen Linux-Computern in Azure. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Azure Key Vault-Instanz
> * Generieren oder Hochladen eines Zertifikats in Key Vault
> * Erstellen eines virtuellen Computers und Installieren des NGINX-Webservers
> * Einfügen des Zertifikats auf dem virtuellen Computer und Konfigurieren von NGINX mit einer SSL-Bindung

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu.  


## <a name="overview"></a>Übersicht
Azure Key Vault schützt Kryptografieschlüssel und geheime Schlüssel, solche Zertifikate oder Kennwörter. Key Vault optimiert die Zertifikatverwaltung und ermöglicht Ihnen, die Kontrolle über Schlüssel zu behalten, die auf diese Zertifikate zugreifen. Sie können ein selbstsigniertes Zertifikat in Key Vault erstellen oder ein vorhandenes vertrauenswürdiges Zertifikat verwenden, das Sie bereits besitzen.

Anstatt ein benutzerdefiniertes VM-Image zu verwenden, in dem die Zertifikate integriert sind, fügen Sie Zertifikate in einen ausgeführten virtuellen Computer ein. Dadurch wird sichergestellt, dass auf einem Webserver während der Bereitstellung die aktuellen Zertifikate installiert sind. Wenn Sie ein Zertifikat erneuern oder ersetzen, müssen Sie nicht auch noch ein neues benutzerdefiniertes VM-Image erstellen. Die neuesten Zertifikate werden beim Erstellen weiterer virtueller Computer automatisch eingefügt. Während des gesamten Prozesses verlässt das Zertifikat nie die Azure-Plattform, noch wird es in einem Skript, Befehlszeilenverlauf oder einer Vorlage verfügbar gemacht.


## <a name="create-an-azure-key-vault"></a>Erstellen einer Azure Key Vault-Instanz
Bevor Sie eine Key Vault-Instanz und Zertifikate erstellen, müssen Sie mit [az group create](/cli/azure/group#create) eine Ressourcengruppe erstellen. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroupSecureWeb* am Standort *eastus* erstellt:

```azurecli-interactive 
az group create --name myResourceGroupSecureWeb --location eastus
```

Erstellen Sie anschließend eine Key Vault-Instanz mit [az keyvault create](/cli/azure/keyvault#create), und aktivieren Sie sie für die Verwendung, wenn Sie einen virtuellen Computer bereitstellen. Jede Key Vault-Instanz benötigt einen eindeutigen Namen, der nur aus Kleinbuchstaben besteht. Ersetzen Sie *<mykeyvault>* im folgenden Beispiel durch Ihren eigenen eindeutigen Key Vault-Namen:

```azurecli-interactive 
keyvault_name=<mykeyvault>
az keyvault create \
    --resource-group myResourceGroupSecureWeb \
    --name $keyvault_name \
    --enabled-for-deployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a>Generieren eines Zertifikats und Speichern in Key Vault
Für die Produktion sollten Sie ein gültiges, von einem vertrauenswürdigen Anbieter signiertes Zertifikat mit [az keyvault certificate import](/cli/azure/certificate#import) importieren. Für dieses Tutorial zeigt das folgende Beispiel, wie Sie ein selbstsigniertes Zertifikat mit [az keyvault certificate create](/cli/azure/certificate#create) generieren können, das die Standardzertifikatrichtlinie verwendet:

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```

### <a name="prepare-a-certificate-for-use-with-a-vm"></a>Vorbereiten eines Zertifikats für die Verwendung mit einem virtuellen Computer
Um das Zertifikat während der Erstellung des virtuellen Computers zu verwenden, rufen Sie die ID des Zertifikats mit [az keyvault secret list-versions](/cli/azure/keyvault/secret#list-versions) ab. Konvertieren Sie das Zertifikat mit [az vm format-secret](/cli/azure/vm#format-secret). Im folgenden Beispiel wird die Ausgabe dieser Befehle Variablen zugewiesen, um die Verwendung in den nächsten Schritten zu vereinfachen:

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```

### <a name="create-a-cloud-init-config-to-secure-nginx"></a>Erstellen einer cloud-init-Konfiguration zum Sichern von NGINX
[Cloud-init](https://cloudinit.readthedocs.io) ist ein weit verbreiteter Ansatz zum Anpassen einer Linux-VM beim ersten Start. Sie können mit cloud-init Pakete installieren und Dateien schreiben oder Benutzer und Sicherheit konfigurieren. Da cloud-init während des ersten Startvorgangs ausgeführt wird, müssen Sie keine zusätzlichen Schritte oder erforderlichen Agents auf Ihre Konfiguration anwenden.

Wenn Sie einen virtuellen Computer erstellen, werden Zertifikate und Schlüssel im geschützten Verzeichnis */var/lib/waagent/* gespeichert. Verwenden Sie cloud-init, um die Vorgänge zum Hinzufügen des Zertifikats zum virtuellen Computer und zum Konfigurieren des Webservers zu automatisieren. In diesem Beispiel wird der NGINX-Webserver installiert und konfiguriert. Dasselbe Verfahren kann zum Installieren und Konfigurieren von Apache verwendet werden. 

Erstellen Sie eine Datei namens *cloud-init-web-server.txt*, und fügen Sie die folgende Konfiguration ein:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 443 ssl;
        ssl_certificate /etc/nginx/ssl/mycert.cert;
        ssl_certificate_key /etc/nginx/ssl/mycert.prv;
      }
runcmd:
  - secretsname=$(find /var/lib/waagent/ -name "*.prv" | cut -c -57)
  - mkdir /etc/nginx/ssl
  - cp $secretsname.crt /etc/nginx/ssl/mycert.cert
  - cp $secretsname.prv /etc/nginx/ssl/mycert.prv
  - service nginx restart
```

### <a name="create-a-secure-vm"></a>Erstellen eines sicheren virtuellen Computers
Jetzt können Sie mit [az vm create](/cli/azure/vm#create) einen virtuellen Computer erstellen. Die Zertifikatsdaten werden mit dem `--secrets`-Parameter aus Key Vault eingefügt. Die cloud-init-Konfiguration wird mit dem Parameter `--custom-data` übergeben:

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-web-server.txt \
    --secrets "$vm_secret"
```

Es dauert einige Minuten, den virtuellen Computer zu erstellen, die Pakete zu installieren und die App zu starten. Sobald der virtuelle Computer erstellt ist, notieren Sie die `publicIpAddress`, die von der Azure-CLI angezeigt wird. Diese Adresse wird verwendet, um über einen Webbrowser auf Ihre Website zuzugreifen.

Damit sicherer Webdatenverkehr Ihren virtuellen Computer erreicht, öffnen Sie Port 443 über das Internet mit [az vm open-port](/cli/azure/vm#open-port):

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --port 443
```


### <a name="test-the-secure-web-app"></a>Testen der sicheren Web-App
Jetzt können Sie einen Webbrowser öffnen und *https://<publicIpAddress>* in die Adressleiste eingeben. Geben Sie Ihre eigene öffentliche IP-Adresse aus dem Erstellungsprozess des virtuellen Computers an. Akzeptieren Sie die Sicherheitswarnung, wenn Sie ein selbstsigniertes Zertifikat verwendet haben:

![Akzeptieren der Webbrowser-Sicherheitswarnung](./media/tutorial-secure-web-server/browser-warning.png)

Die gesicherte NGINX-Website wird dann wie im folgenden Beispiel angezeigt:

![Anzeigen der ausgeführten sicheren NGINX-Website](./media/tutorial-secure-web-server/secured-nginx.png)


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen NGINX-Webserver mit einem in Azure Key Vault gespeicherten SSL-Zertifikat gesichert. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen einer Azure Key Vault-Instanz
> * Generieren oder Hochladen eines Zertifikats in Key Vault
> * Erstellen eines virtuellen Computers und Installieren des NGINX-Webservers
> * Einfügen des Zertifikats auf dem virtuellen Computer und Konfigurieren von NGINX mit einer SSL-Bindung

Folgen Sie diesem Link, um sich vordefinierte Skriptbeispiele für virtuelle Computer anzusehen.

> [!div class="nextstepaction"]
> [Virtueller Azure-Computer – PowerShell-Beispiele](./cli-samples.md)
