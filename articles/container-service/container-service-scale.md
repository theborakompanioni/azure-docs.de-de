---
title: Skalieren Ihres ACS-Clusters mithilfe der Azure-Befehlszeilenschnittstelle | Microsoft-Dokumentation
description: So skalieren Sie Ihren Azure-Containerdienstcluster mithilfe der Azure-Befehlszeilenschnittstelle
services: container-service
documentationcenter: 
author: sauryadas
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: Docker, Container, Microservices, Mesos, Azure
ms.assetid: 4a567474-f9a2-4172-bf86-7522aa4d4d80
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/03/2016
ms.author: saudas
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 9e8df2e68b1b7018d76da89ba9ab332b6ea216fb


---
# <a name="scale-an-azure-container-service"></a>Skalieren eines Azure-Containerdiensts
Sie können die Anzahl der Knoten, über die Ihr Azure-Containerdienst (Azure Container Service, ACS) verfügt, mithilfe des Tools der Azure-Befehlszeilenschnittstelle horizontal hochskalieren. Wenn Sie eine Azure-Befehlszeilenschnittstelle zum Skalieren verwenden, gibt Ihnen das Tool eine neue Konfigurationsdatei zurück, die die angewendeten Änderungen für den Container darstellt.

## <a name="about-the-command"></a>Über den Befehl
Die Azure-Befehlszeilenschnittstelle muss sich im Azure Resource Manager-Modus befinden, damit Sie mit Azure-Containern interagieren können. Sie können in den Ressource Manager-Modus wechseln, indem Sie `azure config mode arm` aufrufen. Der `acs`-Befehl verfügt über einen untergeordneten Befehl mit dem Namen `scale`, der alle Skalierungsvorgänge für einen Containerdienst ausführt. Sie können Hilfe zu den verschiedenen Parametern erhalten, die im Befehl zum Skalieren verwendet werden, indem Sie `azure acs scale --help` ausführen, was eine ähnliche Darstellung wie diese ausgibt:

```azurecli
azure acs scale --help

help:    The operation to scale a container service.
help:
help:    Usage: acs scale [options] <resource-group> <name> <new-agent-count>
help:
help:    Options:
help:      -h, --help                               output usage information
help:      -v, --verbose                            use verbose output
help:      -vv                                      more verbose with debug output
help:      --json                                   use json output
help:      -g, --resource-group <resource-group>    resource-group
help:      -n, --name <name>                        name
help:      -o, --new-agent-count <new-agent-count>  New agent count
help:      -s, --subscription <subscription>        The subscription identifier
help:
help:    Current Mode: arm (Azure Resource Management)
```

## <a name="use-the-command-to-scale"></a>Verwenden des Befehls zum Skalieren
Sie müssen zunächst die **Ressourcengruppe** kennen sowie den **Namen des Azure-Containerdiensts (Azure Container Service, ACS)** und auch die neue Anzahl der Agents angeben, um einen Containerdienst zu skalieren. Indem Sie eine kleinere oder größere Menge verwenden, können Sie jeweils zentral herunter- bzw. hochskalieren.

Möglicherweise möchten Sie die aktuelle Anzahl der Agents für einen Containerdienst wissen, bevor Sie mit der Skalierung beginnen. Verwenden Sie den `azure acs show <resource group> <ACS name>`-Befehl, um die ACS-Konfiguration zurückzugeben. Beachten Sie das Ergebnis <mark>Count</mark>.

#### <a name="see-current-count"></a>Anzeigen der aktuellen Anzahl
```azurecli
azure acs show containers-test containerservice-containers-test

info:    Executing command acs show
data:
data:     Id                 : /subscriptions/<guid>/resourceGroups/containers-test/providers/Microsoft.ContainerService/containerServices/containerservice-containers-test
data:     Name               : containerservice-containers-test
data:     Type               : Microsoft.ContainerService/ContainerServices
data:     Location           : westus
data:     ProvisioningState  : Succeeded
data:     OrchestratorProfile
data:       OrchestratorType : DCOS
data:     MasterProfile
data:       Count            : 1
data:       DnsPrefix        : myprefixmgmt
data:       Fqdn             : myprefixmgmt.westus.cloudapp.azure.com
data:     AgentPoolProfiles
data:       #0
data:         Name           : agentpools
data:         <mark>Count          : 1</mark>
data:         VmSize         : Standard_D2
data:         DnsPrefix      : myprefixagents
data:         Fqdn           : myprefixagents.westus.cloudapp.azure.com
data:     LinuxProfile
data:       AdminUsername    : azureuser
data:       Ssh
data:         PublicKeys
data:           #0
data:             KeyData    : ssh-rsa <ENCODED VALUE>
data:     DiagnosticsProfile
data:       VmDiagnostics
data:         Enabled        : true
data:         StorageUri     : https://<storageid>.blob.core.windows.net/
```  

#### <a name="scale-to-new-count"></a>Skalieren auf die neue Anzahl
Da es wahrscheinlich schon offensichtlich ist, können Sie den Containerdienst skalieren, indem Sie `azure acs scale` aufrufen, und die **Ressourcengruppe**, den **ACS-Namen** und die **Anzahl der Agents** angeben. Wenn Sie einen Containerdienst skalieren, gibt die Azure-Befehlszeilenschnittstelle eine JSON-Zeichenfolge zurück, die die neue Konfiguration des Containerdienst darstellt, einschließlich der neuen Anzahl der Agents.

```azurecli
azure acs scale containers-test containerservice-containers-test 10

info:    Executing command acs scale
data:    {
data:        id: '/subscriptions/<guid>/resourceGroups/containers-test/providers/Microsoft.ContainerService/containerServices/containerservice-containers-test',
data:        name: 'containerservice-containers-test',
data:        type: 'Microsoft.ContainerService/ContainerServices',
data:        location: 'westus',
data:        provisioningState: 'Succeeded',
data:        orchestratorProfile: { orchestratorType: 'DCOS' },
data:        masterProfile: {
data:            count: 1,
data:            dnsPrefix: 'myprefixmgmt',
data:            fqdn: 'myprefixmgmt.westus.cloudapp.azure.com'
data:        },
data:        agentPoolProfiles: [
data:            {
data:                name: 'agentpools',
data:                <mark>count: 10</mark>,
data:                vmSize: 'Standard_D2',
data:                dnsPrefix: 'myprefixagents',
data:                fqdn: 'myprefixagents.westus.cloudapp.azure.com'
data:            }
data:        ],
data:        linuxProfile: {
data:            adminUsername: 'azureuser',
data:            ssh: {
data:                publicKeys: [
data:                    { keyData: 'ssh-rsa <ENCODED VALUE>' }
data:                ]
data:            }
data:        },
data:        diagnosticsProfile: {
data:            vmDiagnostics: { enabled: true, storageUri: 'https://<storageid>.blob.core.windows.net/' }
data:        }
data:    }
info:    acs scale command OK
``` 

## <a name="next-steps"></a>Nächste Schritte
* [Bereitstellen eines Azure Container Service-Clusters](container-service-deployment.md)




<!--HONumber=Nov16_HO3-->


