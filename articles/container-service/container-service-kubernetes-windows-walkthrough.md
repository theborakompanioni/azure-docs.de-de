---
title: "Azure Kubernetes-Cluster für Windows | Microsoft-Dokumentation"
description: "Bereitstellung und erste Schritte für ein Kubernetes-Cluster für Windows-Container in Azure Container Service"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/04/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 18d4994f303a11e9ce2d07bc1124aaedf570fc82
ms.openlocfilehash: 4e730b65a98af05ea00c5f8ebd9914e3367b66a7
ms.contentlocale: de-de
ms.lasthandoff: 05/09/2017


---

# <a name="get-started-with-kubernetes-and-windows-containers-in-container-service"></a>Erste Schritte mit Kubernetes und Windows-Containern in Container Service


Dieser Artikel zeigt Ihnen, wie Sie einen Kubernetes-Cluster in Azure Container Service erstellen, der Windows-Knoten zum Ausführen von Windows-Containern enthält. Beginnen Sie mit der Verwendung von `az acs`-Befehlen von Azure CLI 2.0, um in Azure Container Service einen Kubernetes-Cluster zu erstellen. Verwenden Sie anschließend das `kubectl`-Befehlszeilentool von Kubernetes, damit Sie mit aus Docker-Images erstellten Windows-Containern arbeiten können. 

> [!NOTE]
> Die Unterstützung für Windows-Container mit Kubernetes in Azure Container Service befindet sich in der Vorschauphase. 
>



In der folgenden Abbildung ist die Architektur eines Kubernetes-Clusters in Azure Container Service mit einem Linux-Masterknoten und zwei Windows-Agentknoten dargestellt. 

![Abbildung: Kubernetes-Cluster in Azure](media/container-service-kubernetes-windows-walkthrough/kubernetes-windows.png)

* Der Linux-Master sorgt für die Bereitstellung der Kubernetes-REST-API und ist per SSH über Port 22 oder per `kubectl` über Port 443 zugänglich. 
* Die Windows-Agentknoten werden in einer Azure-Verfügbarkeitsgruppe gruppiert und dienen zum Ausführen Ihrer Container. Auf die Windows-Knoten kann über einen RDP-SSH-Tunnel per Masterknoten zugegriffen werden. Azure-Lastenausgleichsregeln werden dem Cluster je nach den verfügbar gemachten Diensten hinzugefügt.



Alle VMs befinden sich in demselben privaten virtuellen Netzwerk, und untereinander besteht Vollzugriff. Für alle VMs werden ein Kubelet, Docker und ein Proxy ausgeführt.

Weitere Hintergrundinformationen finden Sie in der [Einführung in Docker-Containerhostinglösungen](container-service-intro.md) und der [Kubernetes-Dokumentation](https://kubernetes.io/docs/home/).

## <a name="prerequisites"></a>Voraussetzungen
Zum Erstellen eines Azure Container Service-Clusters mit Azure CLI 2.0 ist Folgendes erforderlich:
* Azure-Konto ([kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/))
* Installation und Anmeldung bei [Azure CLI 2.0](/cli/azure/install-az-cli2)

Sie benötigen für Ihren Kubernetes-Cluster auch Folgendes. Dies können Sie im Voraus vorbereiten oder während der Clusterbereitstellung automatisch mithilfe der `az acs create`-Befehlsoptionen erstellen. 

* **Öffentlicher SSH-RSA-Schlüssel**: Informationen zum Erstellen von Secure Shell (SSH)-RSA-Schlüsseln finden Sie in den Anleitungen für [macOS und Linux](../virtual-machines/linux/mac-create-ssh-keys.md) oder für [Windows](../virtual-machines/linux/ssh-from-windows.md). 

* **Client-ID und Clientgeheimnis des Dienstprinzipals**: Eine Anleitung zum Erstellen eines Azure Active Directory-Dienstprinzipals und weitere Informationen finden Sie unter [Informationen zum Dienstprinzipal für einen Kubernetes-Cluster](container-service-kubernetes-service-principal.md).

Im Beispielbefehl in diesem Artikel werden die SSH-Schlüssel und der Dienstprinzipal automatisch erstellt.
  
## <a name="create-your-kubernetes-cluster"></a>Erstellen Ihres Kubernetes-Clusters

Mithilfe dieser Azure CLI 2.0-Befehle können Sie Ihren Cluster erstellen. 

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Erstellen Sie eine Ressourcengruppe an einem Speicherort, an dem Azure Container Service [verfügbar](https://azure.microsoft.com/regions/services/) ist. Der folgende Befehl erstellt am Standort *westus* eine Ressourcengruppe mit dem Namen *myKubernetesResourceGroup*:

```azurecli
az group create --name=myKubernetesResourceGroup --location=westus
```

### <a name="create-a-kubernetes-cluster-with-windows-agent-nodes"></a>Erstellen eines Kubernetes-Clusters mit Windows-Agentknoten

Erstellen Sie einen Kubernetes-Cluster in Ihrer Ressourcengruppe mithilfe des Befehls `az acs create` mit `--orchestrator-type=kubernetes` und der Agentoption `--windows`. Informationen zur Befehlssyntax finden Sie in der `az acs create`-[Hilfe](/cli/azure/acs#create).

Der folgende Befehl erstellt einen Container Service-Cluster namens *myKubernetesClusterName* mit dem DNS-Präfix *myPrefix* für den Verwaltungsknoten und die angegebenen Anmeldeinformationen, um die Windows-Knoten zu erreichen. Bei dieser Version des Befehls werden die SSH-RSA-Schlüssel und der Dienstprinzipal für den Kubernetes-Cluster automatisch erstellt.


```azurecli
az acs create --orchestrator-type=kubernetes \
    --resource-group myKubernetesResourceGroup \
    --name=myKubernetesClusterName \
    --dns-prefix=myPrefix \
    --agent-count=2 \
    --generate-ssh-keys \
    --windows --admin-username myWindowsAdminName \
    --admin-password myWindowsAdminPassword
```

Nach einigen Minuten ist der Befehl abgeschlossen, und Sie sollten über einen funktionierenden Kubernetes-Cluster verfügen.

> [!IMPORTANT]
> Wenn Ihr Konto nicht über die erforderlichen Berechtigungen zum Erstellen eines Azure AD-Dienstprinzipals verfügt, erzeugt der Befehl einen Fehler wie etwa `Insufficient privileges to complete the operation.`. Weitere Informationen erhalten Sie unter [Informationen zum Dienstprinzipal für einen Kubernetes-Cluster](container-service-kubernetes-service-principal.md). 
> 

## <a name="connect-to-the-cluster-with-kubectl"></a>Herstellen einer Verbindung mit dem Cluster mithilfe von kubectl

Zum Herstellen der Verbindung mit dem Kubernetes-Cluster auf Ihrem Clientcomputer verwenden Sie [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/) (den Kubernetes-Befehlszeilenclient). 

Falls `kubectl` noch nicht lokal installiert wurde, können Sie es mit `az acs kubernetes install-cli` installieren. (Sie können es auch von der [Kubernetes-Website](https://kubernetes.io/docs/tasks/kubectl/install/) herunterladen.)

**Linux oder macOS**

```azurecli
sudo az acs kubernetes install-cli
```

**Windows**
```azurecli
az acs kubernetes install-cli
```

> [!TIP]
> Standardmäßig wird mit diesem Befehl die `kubectl`-Binärdatei auf einem Linux- oder Mac OS-System unter `/usr/local/bin/kubectl` und auf einem Windows-System unter `C:\Program Files (x86)\kubectl.exe` installiert. Um einen anderen Installationspfad anzugeben, können Sie den Parameter `--install-location` verwenden.
>
> Stellen Sie nach der Installation von `kubectl` sicher, dass das dazugehörige Verzeichnis in Ihrem Systempfad enthalten ist, oder fügen Sie es dem Pfad hinzu. 


Führen Sie anschließend den folgenden Befehl aus, um die Masterkonfiguration des Kubernetes-Clusters in die lokale Datei `~/.kube/config` herunterzuladen:

```azurecli
az acs kubernetes get-credentials --resource-group=myKubernetesResourceGroup --name=myKubernetesClusterName
```

Jetzt können Sie von Ihrem Computer auf den Cluster zuzugreifen. Probieren Sie Folgendes aus:

```bash
kubectl get nodes
```

Vergewissern Sie sich, dass eine Liste mit den Computern in Ihrem Cluster angezeigt wird.

![In einem Kubernetes-Cluster ausgeführte Knoten](media/container-service-kubernetes-windows-walkthrough/kubectl-get-nodes.png)

## <a name="create-your-first-kubernetes-service"></a>Erstellen des ersten Kubernetes-Diensts

Nach dem Erstellen des Clusters und dem Herstellen einer Verbindung mit `kubectl` können Sie versuchen, eine Windows-App aus einem Docker-Container zu starten und für den Zugriff über das Internet verfügbar zu machen. In diesem einfachen Beispiel wird mithilfe einer JSON-Datei ein Microsoft-Internetinformationsdienst (IIS)-Container angegeben und mithilfe von `kubctl apply` erstellt. 

1. Erstellen Sie eine lokale Datei mit dem Namen `iis.json`, und kopieren Sie Folgendes. Diese Datei weist Kubernetes an, IIS unter Windows Server 2016 Server Core mithilfe eines öffentlichen Images von [Docker Hub](https://hub.docker.com/r/microsoft/iis/) auszuführen. Der Container verwendet Port 80. Zunächst kann auf ihn aber nur innerhalb des Clusternetzwerks zugegriffen werden.

  ```JSON
  {
    "apiVersion": "v1",
    "kind": "Pod",
    "metadata": {
      "name": "iis",
      "labels": {
        "name": "iis"
      }
    },
    "spec": {
      "containers": [
        {
          "name": "iis",
          "image": "microsoft/iis",
          "ports": [
            {
            "containerPort": 80
            }
          ]
        }
      ],
      "nodeSelector": {
        "beta.kubernetes.io/os": "windows"
      }
    }
  }
  ```
2. Geben Sie Folgendes ein, um die Anwendung zu starten:  
  
  ```bash
  kubectl apply -f iis.json
  ```  
3. Geben Sie Folgendes ein, um die Bereitstellung des Containers nachzuverfolgen:  
  ```bash
  kubectl get pods
  ```
  Während der Bereitstellung des Containers ist der Status `ContainerCreating`. 

  ![IIS-Container im Status „ContainerCreating“](media/container-service-kubernetes-windows-walkthrough/iis-pod-creating.png)   

  Aufgrund der Größe des IIS-Images dauert es unter Umständen einige Minuten, bis der Container in den Status `Running` wechselt.

  ![IIS-Container im Status „Running“](media/container-service-kubernetes-windows-walkthrough/iis-pod-running.png)

4. Geben Sie den folgenden Befehl ein, um den Container global verfügbar zu machen:

  ```bash
  kubectl expose pods iis --port=80 --type=LoadBalancer
  ```

  Dieser Befehl bewirkt, dass Kubernetes eine Azure-Lastenausgleichsregel mit einer öffentlichen IP-Adresse erstellt. Die Weitergabe der Änderung an den Lastenausgleich dauert einige Minuten. Weitere Informationen finden Sie unter [Lastenausgleich für Container in einem Kubernetes-Cluster in Azure Container Service](container-service-kubernetes-load-balancing.md).

5. Führen Sie den folgenden Befehl aus, um den Status des Diensts anzuzeigen.

  ```bash
  kubectl get svc
  ```

  Zunächst wird die IP-Adresse als `pending` angezeigt:

  ![Ausstehende externe IP-Adresse](media/container-service-kubernetes-windows-walkthrough/iis-svc-expose.png)

  Nach einigen Minuten ist die IP-Adresse eingerichtet:
  
  ![Externe IP-Adresse für IIS](media/container-service-kubernetes-windows-walkthrough/iis-svc-expose-public.png)


6. Wenn die externe IP-Adresse verfügbar ist, können Sie im Browser dorthin navigieren:

  ![Abbildung: Navigieren zu IIS](media/container-service-kubernetes-windows-walkthrough/kubernetes-iis.png)  

7. Geben Sie Folgendes ein, um den IIS-Pod zu löschen:

  ```bash
  kubectl delete pods iis
  ```

## <a name="next-steps"></a>Nächste Schritte

* Führen Sie den Befehl `kubectl proxy` aus, um die Kubernetes-Benutzeroberfläche zu verwenden. Navigieren Sie dann zu http://localhost:8001/ui.

* Anweisungen zum Erstellen einer benutzerdefinierten IIS-Website und deren Ausführung in einem Windows-Container finden Sie in [Docker Hub](https://hub.docker.com/r/microsoft/iis/).

* In der [ACS-Engine-Dokumentation](https://github.com/Azure/acs-engine/blob/master/docs/ssh.md#create-port-80-tunnel-to-the-master) erhalten Sie Informationen zum Zugriff auf die Windows-Knoten über einen RDP-SSH-Tunnel zum Master mit PuTTy. 

