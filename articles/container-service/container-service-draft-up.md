---
title: Verwenden von Draft mit Azure Container Service und Azure Container Registry | Microsoft-Dokumentation
description: Erstellen Sie einen ACS Kubernetes-Cluster und eine Azure Container Registry-Instanz, um Ihre erste Anwendung in Azure mit Draft zu erstellen.
services: container-service
documentationcenter: 
author: squillace
manager: gamonroy
editor: 
tags: draft, helm, acs, azure-container-service
keywords: Docker, Container, Microservices, Kubernetes, Draft, Azure
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: rasquill
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: dc3ae52b1ec6717c7e19a160e3e7ea5d211f1f5f
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017



---

<a id="use-draft-with-azure-container-service-and-azure-container-registry-to-build-and-deploy-an-application-to-kubernetes" class="xliff"></a>

# Verwenden von Draft mit Azure Container Service und Azure Container Registry zum Erstellen und Bereitstellen einer Anwendung in Kubernetes

[Draft](https://aka.ms/draft) ist ein neues Open-Source-Tool, mit dem containerbasierte Anwendungen auf einfache Weise entwickelt und in Kubernetes-Clustern bereitgestellt werden können, ohne dass umfassende Kenntnisse über Docker und Kubernetes erforderlich sind oder diese installiert werden müssen. Durch Verwendung von Tools wie Draft können Sie und Ihre Teams sich auf die Erstellung der Anwendung mit Kubernetes konzentrieren und der Infrastruktur dabei etwas weniger Beachtung schenken.

Sie können Draft mit sämtlichen Docker-Imageregistrierungen und sämtlichen Kubernetes-Clustern – und auch lokal – verwenden. In diesem Tutorial wird erläutert, wie Sie unter Verwendung von ACS mit Kubernetes, ACR und Azure DNS eine Live-CI/CD-Entwicklerpipeline mithilfe von Draft erstellen.


<a id="create-an-azure-container-registry" class="xliff"></a>

## Erstellen einer Azure-Containerregistrierung
Sie können [eine neue Azure Container Registry-Instanz](../container-registry/container-registry-get-started-azure-cli.md) mühelos mit den folgenden Schritten erstellen:

1. Erstellen Sie eine Azure-Ressourcengruppe zum Verwalten der ACR-Registrierung und des Kubernetes-Clusters in ACS.
      ```azurecli
      az group create --name draft --location eastus
      ```

2. Erstellen Sie mit [az acr create](/cli/azure/acr#create) eine ACR-Imageregistrierung.
      ```azurecli
      az acr create -g draft -n draftacs --sku Basic --admin-enabled true -l eastus
      ```


<a id="create-an-azure-container-service-with-kubernetes" class="xliff"></a>

## Erstellen einer Azure Container Service-Instanz mit Kubernetes

Nun können Sie mit [az acs create](/cli/azure/acs#create) einen ACS-Cluster mit „Kubernetes“ als Wert für `--orchestrator-type` erstellen.
```azurecli
az acs create --resource-group draft --name draft-kube-acs --dns-prefix draft-cluster --orchestrator-type kubernetes
```

> [!NOTE]
> Da Kubernetes nicht der standardmäßige Orchestratortyp ist, müssen Sie den Switch `--orchestrator-type kubernetes` verwenden.

Wenn erfolgreich, sieht die Ausgabe in etwa wie folgt aus.

```json
waiting for AAD role to propagate.done
{
  "id": "/subscriptions/<guid>/resourceGroups/draft/providers/Microsoft.Resources/deployments/azurecli14904.93snip09",
  "name": "azurecli1496227204.9323909",
  "properties": {
    "correlationId": "<guid>",
    "debugSetting": null,
    "dependencies": [],
    "mode": "Incremental",
    "outputs": null,
    "parameters": {
      "clientSecret": {
        "type": "SecureString"
      }
    },
    "parametersLink": null,
    "providers": [
      {
        "id": null,
        "namespace": "Microsoft.ContainerService",
        "registrationState": null,
        "resourceTypes": [
          {
            "aliases": null,
            "apiVersions": null,
            "locations": [
              "westus"
            ],
            "properties": null,
            "resourceType": "containerServices"
          }
        ]
      }
    ],
    "provisioningState": "Succeeded",
    "template": null,
    "templateLink": null,
    "timestamp": "2017-05-31T10:46:29.434095+00:00"
  },
  "resourceGroup": "draft"
}
```

Damit verfügen Sie über einen Cluster und können nun mit dem Befehl [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) die Anmeldeinformationen importieren. Damit verfügen Sie über eine lokale Konfigurationsdatei für den Cluster. Diese ist erforderlich, damit Helm und Draft ausgeführt werden können.

<a id="install-and-configure-draft" class="xliff"></a>

## Installieren und Konfigurieren von Draft
Die Installationsanweisungen für Draft finden Sie im [Draft-Repository](https://github.com/Azure/draft/blob/master/docs/install.md). Diese sind relativ einfach, erfordern dennoch einige Konfigurationsschritte, da [Helm](https://aka.ms/helm) für die Erstellung und Bereitstellung eines Helm-Diagramms im Kubernetes-Cluster erforderlich ist.

1. [Laden Sie Helm herunter, und installieren Sie das Tool](https://aka.ms/helm#install).
2. Verwenden Sie Helm zum Suchen und Installieren von `stable/traefik` und einen Eingangscontroller, um eingehende Anforderungen für Ihre Builds zu ermöglichen.
    ```bash
    $ helm search traefik
    NAME            VERSION DESCRIPTION
    stable/traefik  1.3.0   A Traefik based Kubernetes ingress controller w...

    $ helm install stable/traefik --name ingress
    ```
    Legen Sie eine Überwachung für den `ingress`-Controller fest, um bei dessen Bereitstellung den externen IP-Wert zu erfassen. Bei dieser IP-Adresse handelt es sich um die Adresse, die im nächsten Abschnitt [Ihrer Bereitstellungsdomäne zugeordnet wird](#wire-up-deployment-domain).

    ```bash
    kubectl get svc -w
    NAME                          CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
    ingress-traefik               10.0.248.104   13.64.108.240   80:31046/TCP,443:32556/TCP   1h
    kubernetes                    10.0.0.1       <none>          443/TCP                      7h
    ```

    In diesem Fall lautet die externe IP für die Bereitstellungsdomäne `13.64.108.240`. Sie können nun Ihre Domäne dieser IP zuordnen.

<a id="wire-up-deployment-domain" class="xliff"></a>

## Einrichten der Bereitstellungsdomäne

Draft erstellt eine Version für jedes erstellte Helm-Diagramm, d.h. jede Anwendung, an der Sie arbeiten. Jeder wird ein generierter Name zugewiesen, der in Draft als _Unterdomäne_ verwendet wird, zusätzlich zu der von Ihnen gesteuerten _Stammbereitstellungsdomäne_. (In diesem Beispiel wird `squillace.io` als Bereitstellungsdomäne verwendet.) Um dieses Unterdomänenverhalten zu ermöglichen, müssen Sie einen A-Datensatz für `'*'` in den DNS-Einträgen für die Bereitstellungsdomäne erstellen, sodass jede generierte Unterdomäne an den Eingangscontroller des Kubernetes-Clusters weitergeleitet wird.

Ihr Domänenanbieter weist DNS-Server auf eigene Weise zu. Um [Ihre Domänennamenserver an Azure DNS zu delegieren](../dns/dns-delegate-domain-azure-dns.md), gehen Sie wie folgt vor:

1. Erstellen Sie eine Ressourcengruppe für Ihre Zone.
    ```azurecli
    az group create --name squillace.io --location eastus
    {
      "id": "/subscriptions/<guid>/resourceGroups/squillace.io",
      "location": "eastus",
      "managedBy": null,
      "name": "zones",
      "properties": {
        "provisioningState": "Succeeded"
      },
      "tags": null
    }
    ```

2. Erstellen Sie eine DNS-Zone für Ihre Domäne.
Verwenden Sie den Befehl [az network dns zone create](/cli/azure/network/dns/zone#create) zum Abrufen der Namenserver, für die die DNS-Steuerung für Ihre Domäne an Azure DNS delegiert wird.
    ```azurecli
    az network dns zone create --resource-group squillace.io --name squillace.io
    {
      "etag": "<guid>",
      "id": "/subscriptions/<guid>/resourceGroups/zones/providers/Microsoft.Network/dnszones/squillace.io",
      "location": "global",
      "maxNumberOfRecordSets": 5000,
      "name": "squillace.io",
      "nameServers": [
        "ns1-09.azure-dns.com.",
        "ns2-09.azure-dns.net.",
        "ns3-09.azure-dns.org.",
        "ns4-09.azure-dns.info."
      ],
      "numberOfRecordSets": 2,
      "resourceGroup": "squillace.io",
      "tags": {},
      "type": "Microsoft.Network/dnszones"
    }
    ```
3. Fügen Sie die abgerufenen DNS-Server dem Domänenanbieter für Ihre Bereitstellungsdomäne hinzu. So können Sie mithilfe von Azure DNS Ihre Domäne nach Wunsch umleiten.
4. Erstellen Sie einen A-Datensatzeintrag für Ihre Bereitstellungsdomäne, der der `ingress`-IP aus Schritt 2 des vorherigen Abschnitts zugeordnet ist.
    ```azurecli
    az network dns record-set a add-record --ipv4-address 13.64.108.240 --record-set-name '*' -g squillace.io -z squillace.io
    ```
Die Ausgabe sieht in etwa wie folgt aus:
    ```json
    {
      "arecords": [
        {
          "ipv4Address": "13.64.108.240"
        }
      ],
      "etag": "<guid>",
      "id": "/subscriptions/<guid>/resourceGroups/squillace.io/providers/Microsoft.Network/dnszones/squillace.io/A/*",
      "metadata": null,
      "name": "*",
      "resourceGroup": "squillace.io",
      "ttl": 3600,
      "type": "Microsoft.Network/dnszones/A"
    }
    ```

5. Konfigurieren Sie Draft so, dass Ihre Registrierung verwendet wird und für jedes erstellte Helm-Diagramm Unterdomänen erstellt werden. Für die Konfiguration von Draft benötigen Sie Folgendes:
  - Ihren Azure Container Registry-Namen (in diesem Beispiel `draft`)
  - Ihren Registrierungsschlüssel oder Ihr Kennwort aus `az acr credential show -n <registry name> --output tsv --query "passwords[0].value"`
  - die Stammbereitstellungsdomäne, die Sie für die Zuordnung zur externen IP-Adresse des Kubernetes-Eingangs konfiguriert haben (in diesem Fall `squillace.io`)

  Rufen Sie `draft init` auf. Beim Konfigurationsvorgang werden Sie zur Eingabe der oben genannten Werte aufgefordert. Der Prozess sieht bei der ersten Ausführung etwa wie folgt aus:
    ```
    draft init
    Creating pack ruby...
    Creating pack node...
    Creating pack gradle...
    Creating pack maven...
    Creating pack php...
    Creating pack python...
    Creating pack dotnetcore...
    Creating pack golang...
    $DRAFT_HOME has been configured at /Users/ralphsquillace/.draft.

    In order to install Draft, we need a bit more information...

    1. Enter your Docker registry URL (e.g. docker.io, quay.io, myregistry.azurecr.io): draft.azurecr.io
    2. Enter your username: draft
    3. Enter your password:
    4. Enter your org where Draft will push images [draft]: draft
    5. Enter your top-level domain for ingress (e.g. draft.example.com): squillace.io
    Draft has been installed into your Kubernetes Cluster.
    Happy Sailing!
    ```

Nun können Sie eine Anwendung bereitstellen.


<a id="build-and-deploy-an-application" class="xliff"></a>

## Erstellen und Bereitstellen einer Anwendung

Das Draft-Repository enthält [sechs einfache Beispielanwendungen](https://github.com/Azure/draft/tree/master/examples). Klonen Sie das Repository, und verwenden Sie das [Python-Beispiel](https://github.com/Azure/draft/tree/master/examples/python). Wechseln Sie in das Verzeichnis „examples/Python“, und geben Sie `draft create` ein, um die Anwendung zu erstellen. Sie sollte wie das folgende Beispiel aussehen.
```bash
$ draft create
--> Python app detected
--> Ready to sail
```

Die Ausgabe enthält eine Dockerfile-Datei und ein Helm-Diagramm. Für die Erstellung und Bereitstellung geben Sie einfach `draft up` ein. Die Ausgabe ist umfangreich und beginnt wie im folgenden Beispiel.
```bash
$ draft up
--> Building Dockerfile
Step 1 : FROM python:onbuild
onbuild: Pulling from library/python
10a267c67f42: Pulling fs layer
fb5937da9414: Pulling fs layer
9021b2326a1e: Pulling fs layer
dbed9b09434e: Pulling fs layer
ea8a37f15161: Pulling fs layer
<snip>
```

Bei erfolgreichem Abschluss, endet sie in etwa wie das folgende Beispiel.
```bash
ab68189731eb: Pushed
53c0ab0341bee12d01be3d3c192fbd63562af7f1: digest: sha256:bb0450ec37acf67ed461c1512ef21f58a500ff9326ce3ec623ce1e4427df9765 size: 2841
--> Deploying to Kubernetes
--> Status: DEPLOYED
--> Notes:

  http://gangly-bronco.squillace.io to access your application

Watching local files for changes...
```

Unabhängig vom Namen des Diagramms erhalten Sie mit `curl http://gangly-bronco.squillace.io` nun die Antwort `Hello World!`.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte

Sie haben einen ACS Kubernetes-Cluster erstellt und können nun mithilfe von [Azure Container Registry](../container-registry/container-registry-intro.md) weitere verschiedene Bereitstellungen dieses Szenarios erstellen. Beispielsweise können Sie einen _draft.basedomain.toplevel_-Domänen-DNS-Datensatz erstellen, mit dem Elemente einer Unterdomäne auf tieferer Ebene für bestimmte ACS-Bereitstellungen gesteuert werden.







