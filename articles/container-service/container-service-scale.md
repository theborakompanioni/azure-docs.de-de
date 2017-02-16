---
title: Skalieren eines Azure Container Service-Clusters | Microsoft-Dokumentation
description: "So skalieren Sie Ihren Azure Container Service-Cluster mithilfe der Azure-Befehlszeilenschnittstelle oder über das Azure-Portal."
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
ms.date: 01/10/2017
ms.author: saudas
translationtype: Human Translation
ms.sourcegitcommit: cb3fd28659eb09dfb74496d2aa526736d223631a
ms.openlocfilehash: d1571aa6191111c46c43b3a424cea415091adfc9


---
# <a name="scale-an-azure-container-service-cluster"></a>Skalieren eines Azure Container Service-Clusters
Nach dem [Bereitstellen eines Azure Container Service-Clusters](container-service-deployment.md) müssen Sie eventuell die Anzahl von Agent-Knoten ändern. Möglicherweise benötigen Sie mehr Agents, damit Sie mehr Containeranwendungen oder -instanzen ausführen können. 

Sie können die Anzahl von Agent-Knoten im Cluster über das Azure-Portal oder mithilfe der Azure-CLI 2.0 (Vorschau) ändern. Die Azure-CLI 2.0 (Vorschau) ist die [Befehlszeilenschnittstelle der nächsten Generation](/cli/azure/old-and-new-clis) für das Resource Manager-Bereitstellungsmodell.

> [!NOTE]
> Zurzeit wird die Skalierung von Agent-Knoten in einem Kubernetes-Containerdienstcluster nicht unterstützt.


## <a name="scale-with-the-azure-portal"></a>Skalieren über das Azure-Portal

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach **Containerdiensten**, und klicken Sie dann auf den Containerdienst, den Sie ändern möchten.
2. Klicken Sie auf dem Blatt **Containerdienst** auf **Agents**.
3. Geben Sie unter **VM-Anzahl** die gewünschte Anzahl von Agent-Knoten ein.

    ![Skalieren eines Pools im Portal](./media/container-service-scale/container-service-scale-portal.png)

4. Klicken Sie zum Speichern der Konfiguration auf **Speichern**.



## <a name="scale-with-the-azure-cli-20-preview"></a>Skalieren mit der Azure-CLI 2.0 (Vorschau)

Stellen Sie sicher, dass Sie die aktuelle Version der Azure-CLI 2.0 (Vorschau) [installiert](/cli/azure/install-az-cli2) haben und an einem Azure-Konto (`az login`) angemeldet sind.


### <a name="see-the-current-agent-count"></a>Anzeigen der aktuellen Agent-Anzahl
Um die Anzahl der Agents anzuzeigen, die sich derzeit im Cluster befinden, führen Sie den Befehl `az acs show` aus. Damit wird die Clusterkonfiguration angezeigt. Der folgende Befehl zeigt z.B. die Konfiguration des Containerdiensts namens `containerservice-myACSName` in der Ressourcengruppe `myResourceGroup`:

```azurecli
az acs show -g myResourceGroup -n containerservice-myACSName
```

Der Befehl gibt die Anzahl von Agents im `Count`-Wert unter `AgentPoolProfiles` zurück.


### <a name="use-the-az-acs-scale-command"></a>Verwenden des Befehls „az acs scale“
Um die Anzahl von Agent-Knoten zu ändern, führen Sie den `az acs scale`-Befehl aus, und geben Sie die **Ressourcengruppe**, den **Namen des Containerdiensts** und die gewünschte **neue Agent-Anzahl** an. Durch Verwendung einer kleineren oder größeren Zahl können Sie jeweils zentral herunter- bzw. hochskalieren.

Um z.B. die Anzahl von Agents im vorherigen Cluster auf 10 zu ändern, geben Sie folgenden Befehl ein:

```azurecli
azure acs scale -g myResourceGroup -n containerservice-myACSName --new-agent-count 10
```

Die Azure-CLI 2.0 (Vorschau) gibt eine JSON-Zeichenfolge zurück, die die neue Konfiguration des Containerdiensts darstellt, einschließlich der neuen Anzahl von Agents.

Um weitere Befehlsoptionen zu erhalten, führen Sie `az acs scale --help` aus.


## <a name="scaling-considerations"></a>Überlegungen zur Skalierung


* Die Anzahl von Agent-Knoten muss zwischen 1 und 100 (einschließlich) liegen. 

* Das Kernnutzungskontingent Ihres Systems kann die Anzahl von Agent-Knoten in einem Cluster begrenzen.

* Skalierungsvorgänge für Azure-Knoten werden auf eine Azure-VM-Skalierungsgruppe angewendet, die den Agent-Pool enthält. In einem DC/OS-Cluster werden nur Agent-Knoten im privaten Pool von den in diesem Artikel gezeigten Vorgängen skaliert.

* Je nach der im Cluster bereitgestellten Orchestrierung können Sie die Anzahl von Instanzen eines im Cluster ausgeführten Containers separat skalieren. Verwenden Sie in einem DC/OS-Cluster z.B. die [Marathon-Schnittstelle](container-service-mesos-marathon-ui.md), um die Anzahl von Instanzen einer Containeranwendung zu ändern.

* Zurzeit wird die automatische Skalierung von Agent-Knoten in einem Containerdienstcluster nicht unterstützt.





## <a name="next-steps"></a>Nächste Schritte
* Hier finden Sie [weitere Beispiele](container-service-create-acs-cluster-cli.md) für die Verwendung von Befehlen der Azure-CLI 2.0 (Vorschau) mit Azure Container Service.
* Hier erfahren Sie mehr über [DC/OS-Agent-Pools](container-service-dcos-agents.md) in Azure Container Service.




<!--HONumber=Jan17_HO2-->


