---
title: Azure Container Instances-Gruppe mit mehreren Containern | Azure-Dokumentation
description: Azure Container Instances-Gruppe mit mehreren Containern
services: container-instances
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: nepeters
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: 94744e8138d40dd777c0c004472804e3af6c0b1e
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---

# <a name="deploy-a-container-group"></a>Bereitstellen einer Containergruppe

Azure Container Instances unterstützt die Bereitstellung von mehreren Containern auf einem einzelnen Host mit einer *Containergruppe*. Dies ist nützlich, wenn eine Sidecar-Anwendung für die Protokollierung, die Überwachung oder irgendeine andere Konfiguration, bei der ein Dienst einen zweiten angefügten Prozess benötigt, erstellt wird. 

In diesem Artikel wird das Ausführen einer einfachen Sidecar-Konfiguration mit mehreren Containern mithilfe einer Azure Resource Manager-Vorlage beschrieben.

## <a name="configure-the-template"></a>Konfigurieren der Vorlage

Erstellen Sie eine Datei mit dem Namen `azuredeploy.json`, und fügen Sie das folgende JSON-Dokument ein. 

In diesem Beispiel wird eine Containergruppe mit zwei Containern und einer öffentlichen IP-Adresse definiert. Der erste Container der Gruppe führt eine Anwendung mit Internetverbindung aus. Der zweite Container, der Sidecar, stellt eine HTTP-Anforderung an die Hauptwebanwendung über das lokale Netzwerk der Gruppe. 

Dieses Sidecar-Beispiel kann erweitert werden, um eine Warnung auszulösen, wenn ein anderer HTTP-Antwortcode als 200 OK empfangen wird. 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
  },
  "variables": {
    "container1name": "aci-tutorial-app",
    "container1image": "microsoft/aci-helloworld:latest",
    "container2name": "aci-tutorial-sidecar",    
    "container2image": "microsoft/aci-tutorial-sidecar"
  },
    "resources": [
      {
        "name": "myContainerGroup",
        "type": "Microsoft.ContainerInstance/containerGroups",
        "apiVersion": "2017-08-01-preview",
        "location": "[resourceGroup().location]",
        "properties": {
          "containers": [
            {
              "name": "[variables('container1name')]",
              "properties": {
                "image": "[variables('container1image')]",
                "resources": {
                  "requests": {
                    "cpu": 1,
                    "memoryInGb": 1.5
                    }
                },
                "ports": [
                  {
                    "port": 80
                  }
                ]
              }
            },
            {
              "name": "[variables('container2name')]",
              "properties": {
                "image": "[variables('container2image')]",
                "resources": {
                  "requests": {
                    "cpu": 1,
                    "memoryInGb": 1.5
                    }
                }
              }
            }
          ],
          "osType": "Linux",
          "ipAddress": {
            "type": "Public",
            "ports": [
              {
                "protocol": "tcp",
                "port": "80"
              }
            ]
          }
        }
      }
    ],
    "outputs": {
      "containerIPv4Address": {
        "type": "string",
        "value": "[reference(resourceId('Microsoft.ContainerInstance/containerGroups/', 'myContainerGroup')).ipAddress.ip]"
      }
    }
  }
```

Fügen Sie ein Objekt zum JSON-Dokument mit dem folgenden Format hinzu, um einen Container der privaten Imageregistrierung verwenden zu können.

```json
"imageRegistryCredentials": [
    {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
    }
]
```

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine Ressourcengruppe.

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

Stellen Sie mit dem Befehl [az group deployment create](/cli/azure/group/deployment#create) die Vorlage bereit.

```azurecli-interactive
az group deployment create --name myContainerGroup --resource-group myResourceGroup --template-file azuredeploy.json
```

Innerhalb weniger Sekunden erhalten Sie eine erste Antwort von Azure. 

## <a name="view-deployment-state"></a>Zusammenfassung der Bereitstellungen anzeigen

Verwenden Sie zum Anzeigen des Status der Bereitstellung den `az container show`-Befehl. Dieser gibt die bereitgestellte öffentliche IP-Adresse zurück, über die auf die Anwendung zugegriffen werden kann.

```azurecli-interactive
az container show --name myContainerGroup --resource-group myResourceGroup -o table
```

Ausgabe:

```azurecli
Name              ResourceGroup    ProvisioningState    Image                                                             IP:ports           CPU/Memory    OsType    Location
----------------  ---------------  -------------------  ----------------------------------------------------------------  -----------------  ------------  --------  ----------
myContainerGroup  myResourceGrou2  Succeeded            microsoft/aci-tutorial-sidecar,microsoft/aci-tutorial-app:v1      40.118.253.154:80  1.0 core/1.5 gb   Linux     westus
```

## <a name="view-logs"></a>Anzeigen von Protokollen   

Zeigen Sie die Protokollausgabe eines Containers mit dem `az container logs`-Befehl an. Das `--container-name`-Argument gibt den Container an, aus dem Protokolle erhalten werden können. In diesem Beispiel wird der erste Container angegeben. 

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-app --resource-group myResourceGroup
```

Ausgabe:

```bash
istening on port 80
::1 - - [27/Jul/2017:17:35:29 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:32 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:35 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:38 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

Führen Sie den gleichen Befehl aus, und geben Sie den zweiten Containernamen an, um die Protokolle für den Sidecar-Container anzuzeigen.

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-sidecar --resource-group myResourceGroup
```

Ausgabe:

```bash
Every 3.0s: curl -I http://localhost                                                                                                                       Mon Jul 17 11:27:36 2017

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0  0  1663    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
Accept-Ranges: bytes
Content-Length: 1663
Content-Type: text/html; charset=utf-8
Last-Modified: Sun, 16 Jul 2017 02:08:22 GMT
Date: Mon, 17 Jul 2017 18:27:36 GMT
```

Wie Sie sehen können, erfolgt vom Sidecar in regelmäßigen Abständen eine HTTP-Anforderung an die Hauptwebanwendung über das lokale Netzwerk der Gruppe, um sicherzustellen, dass er ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte

Dieses Dokument behandelt die für die Bereitstellung einer Azure-Instanz mit mehreren Containern erforderlichen Schritte. Eine End-to-End-Erfahrung mit Azure Container Instances finden Sie im Azure Container Instances-Tutorial.

> [!div class="nextstepaction"]
> [Azure Container Instances-Tutorial]: ./container-instances-tutorial-prepare-app.md

