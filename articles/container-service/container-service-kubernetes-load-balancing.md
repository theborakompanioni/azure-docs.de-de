---
title: "Lastenausgleich für Kubernetes-Container in Azure | Microsoft-Dokumentation"
description: "Stellen Sie eine externe Verbindung her, und führen Sie einen Lastenausgleich für mehrere Container in einem Kubernetes-Cluster in Azure Container Service aus."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Container, Microservices, Kubernetes, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: danlep
ms.translationtype: Human Translation
ms.sourcegitcommit: 95b8c100246815f72570d898b4a5555e6196a1a0
ms.openlocfilehash: 9046879158a4617d478bcf1157d5ead3c1054fd8
ms.contentlocale: de-de
ms.lasthandoff: 05/18/2017


---
# <a name="load-balance-containers-in-a-kubernetes-cluster-in-azure-container-service"></a>Lastenausgleich für Container in einem Kubernetes-Cluster in Azure Container Service 
Dieser Artikel behandelt den Lastenausgleich für einen Kubernetes-Cluster in Azure Container Service. Der Lastenausgleich stellt eine extern aufrufbare IP-Adresse für den Dienst bereit und verteilt den Netzwerkdatenverkehr auf die Pods, die auf Agent-VMs ausgeführt werden.

Sie können einen Kubernetes-Dienst so einrichten, dass er [Azure Load Balancer](../load-balancer/load-balancer-overview.md) zum Verwalten des externen Netzwerkdatenverkehrs (TCP) verwendet. Mit der zusätzlichen Konfiguration sind der Lastenausgleich und die Weiterleitung von HTTP- oder HTTPS-Datenverkehr oder komplexere Szenarien möglich.

## <a name="prerequisites"></a>Voraussetzungen
* [Bereitstellen eines Kubernetes-Clusters](container-service-kubernetes-walkthrough.md) in Azure Container Service
* [Verbinden des Clients](container-service-connect.md) mit dem Cluster

## <a name="azure-load-balancer"></a>Azure Load Balancer

Standardmäßig enthält ein in Azure Container Service bereitgestellter Kubernetes-Cluster einen Azure-Lastenausgleich mit Internetzugriff für die Agent-VMs. (Eine separate Lastenausgleichsressource ist für die Master-VMs konfiguriert.) Azure Load Balancer ist eine Layer-4-Lastenausgleichskomponente. Derzeit unterstützt Load Balancer nur TCP-Datenverkehr in Kubernetes.

Beim Erstellen eines Kubernetes-Diensts können Sie Azure Load Balancer automatisch so konfigurieren, dass er den Zugriff auf den Dienst erlaubt. Zum Konfigurieren des Lastenausgleichs setzen Sie `type` des Diensts auf `LoadBalancer`. Der Lastenausgleich erstellt eine Regel zum Zuordnen einer IP-Adresse und einer Portnummer des eingehenden Dienstdatenverkehrs zu den privaten IP-Adressen und Portnummern der Pods in Agent-VMs (und umgekehrt für den Antwortdatenverkehr). 

 Es folgen zwei Beispiele zum Einstellen von `type` des Kubernetes-Diensts auf `LoadBalancer`. (Nach dem Testen der Beispiele löschen Sie die Bereitstellungen, wenn Sie diese nicht mehr benötigen.)

### <a name="example-use-the-kubectl-expose-command"></a>Beispiel: Verwenden des `kubectl expose`-Befehls 
[Kubernetes walkthrough](container-service-kubernetes-walkthrough.md) (Kubernetes – exemplarische Vorgehensweise) enthält ein Beispiel zum Bereitstellen eines Diensts mit dem `kubectl expose`-Befehl und seinem `--type=LoadBalancer`-Flag. Gehen Sie wie folgt vor:

1. Starten Sie eine neue Containerbereitstellung. Beispiel: Der folgende Befehl startet eine neue Bereitstellung mit der Bezeichnung `mynginx`. Die Bereitstellung besteht aus drei Containern auf Grundlage des Docker-Images für den Nginx-Webserver.

    ```console
    kubectl run mynginx --replicas=3 --image nginx
    ```
2. Stellen Sie sicher, dass die Container ausgeführt werden. Beispiel: Falls Sie nach den Containern mit `kubectl get pods` suchen, sehen Sie in etwa folgendes Ergebnis:

    ![Abrufen von Nginx-Containern](./media/container-service-kubernetes-load-balancing/nginx-get-pods.png)

3. Zum Konfigurieren des Lastenausgleichs zur Aufnahme von externem Datenverkehr in die Bereitstellung müssen Sie `kubectl expose` mit `--type=LoadBalancer` ausführen. Der folgende Befehl stellt den Nginx-Server an Port 80 zur Verfügung:

    ```console
    kubectl expose deployments mynginx --port=80 --type=LoadBalancer
    ```

4. Geben Sie `kubectl get svc` ein, um den Status der Dienste im Cluster anzuzeigen. Während der Lastenausgleich die Regel konfiguriert, wird `EXTERNAL-IP` des Diensts als `<pending>` angezeigt. Nach ein paar Minuten ist die externe IP-Adresse konfiguriert: 

    ![Konfigurieren von Azure Load Balancer](./media/container-service-kubernetes-load-balancing/nginx-external-ip.png)

5. Stellen Sie sicher, dass Sie den Dienst unter der externen IP-Adresse aufrufen können. Beispiel: Öffnen Sie in einem Webbrowser die angezeigte IP-Adresse. Der Browser zeigt den Nginx-Webserver, der in einem der Container ausgeführt wird. Führen Sie alternativ den Befehl `curl` oder `wget` aus. Beispiel:

    ```
    curl 13.82.93.130
    ```

    Eine ähnliche Ausgabe wie die folgende sollte angezeigt werden:

    ![Aufrufen von Nginx mit Curl](./media/container-service-kubernetes-load-balancing/curl-output.png)

6. Zum Anzeigen der Konfiguration von Azure Load Balancer wechseln Sie in das [Azure-Portal](https://portal.azure.com).

7. Suchen Sie nach der Ressourcengruppe für den Containerdienst-Cluster, und wählen Sie den Lastenausgleich für die Agent-VMs aus. Der Name muss dem des Containerdiensts entsprechen. (Wählen Sie keinen Lastenausgleich für die Masterknoten, deren Namen **master-lb** enthalten.) 

    ![Lastenausgleich in der Ressourcengruppe](./media/container-service-kubernetes-load-balancing/container-resource-group-portal.png)

8. Klicken Sie zum Anzeigen der Details der Konfiguration des Lastenausgleichs auf **Load balancing rules** (Lastenausgleichsregeln) und den Namen der Regel, die konfiguriert wurde.

    ![Lastenausgleichsregeln](./media/container-service-kubernetes-load-balancing/load-balancing-rules.png) 

### <a name="example-specify-type-loadbalancer-in-the-service-configuration-file"></a>Beispiel: Geben Sie in der Dienstkonfigurationsdatei `type: LoadBalancer` an.

Stellen Sie eine Kubernetes-Container-App aus einer YAML- oder JSON-[Dienstkonfigurationsdatei](https://kubernetes.io/docs/user-guide/services/operations/#service-configuration-file) bereit, müssen Sie einen externen Lastenausgleich angeben, indem Sie die folgende Zeile zur Dienstspezifikation hinzufügen:

```YAML
 "type": "LoadBalancer"
``` 



Die folgenden Schritte verwenden das [Guestbook example](https://github.com/kubernetes/kubernetes/tree/master/examples/guestbook) (Gästebuchbeispiel) von Kubernetes. Bei diesem Beispiel handelt es sich um eine Web-App mit mehreren Ebenen, die auf Redis- und PHP-Docker-Images basiert. Sie können in der Dienstkonfigurationsdatei angeben, dass der Front-End-PHP-Server Azure Load Balancer verwendet.

1. Laden Sie die Datei `guestbook-all-in-one.yaml` aus [GitHub](https://github.com/kubernetes/kubernetes/tree/master/examples/guestbook/all-in-one) herunter. 
2. Suchen Sie nach `spec` für den `frontend`-Dienst.
3. Löschen Sie den Kommentar in der Zeile `type: LoadBalancer`.

    ![Lastenausgleich in der Dienstkonfiguration](./media/container-service-kubernetes-load-balancing/guestbook-frontend-loadbalance.png)

4. Speichern Sie die Datei, und stellen Sie die App bereit, indem Sie den folgenden Befehl ausführen:

    ```
    kubectl create -f guestbook-all-in-one.yaml
    ```

5. Geben Sie `kubectl get svc` ein, um den Status der Dienste im Cluster anzuzeigen. Während der Lastenausgleich die Regel konfiguriert, wird `EXTERNAL-IP` des `frontend`-Diensts als `<pending>` angezeigt. Nach ein paar Minuten ist die externe IP-Adresse konfiguriert: 

    ![Konfigurieren von Azure Load Balancer](./media/container-service-kubernetes-load-balancing/guestbook-external-ip.png)

6. Stellen Sie sicher, dass Sie den Dienst unter der externen IP-Adresse aufrufen können. Beispiel: Sie können in einem Webbrowser die externe IP-Adresse des Diensts öffnen.

    ![Externes Aufrufen des Gästebuchs](./media/container-service-kubernetes-load-balancing/guestbook-web.png)

    Sie können Gästebucheinträge hinzufügen.

7. Zum Anzeigen der Konfiguration von Azure Load Balancer suchen Sie die Lastenausgleichsressource für den Cluster im [Azure-Portal](https://portal.azure.com). Betrachten Sie dazu auch die Schritte im vorherigen Beispiel.

### <a name="considerations"></a>Überlegungen

* Die Erstellung der Lastenausgleichsregel erfolgt asynchron. Die Informationen zum bereitgestellten Lastenausgleich werden im `status.loadBalancer`-Feld des Diensts veröffentlicht.
* Jedem Dienst wird automatisch eine eigene virtuelle IP-Adresse im Lastenausgleich zugewiesen.
* Wenn Sie den Lastenausgleich anhand eines DNS-Namens erreichen möchten, erstellen Sie gemeinsam mit Ihrem Domänendienstanbieter einen DNS-Namen für die IP-Adresse der Regel.

## <a name="http-or-https-traffic"></a>HTTP- oder HTTPS-Datenverkehr

Zum Lastenausgleich von HTTP- oder HTTPS-Datenverkehr an die Container-Web-Apps und zur Verwaltung von Zertifikaten für Transport Layer Security (TLS) verwenden Sie die Kubernetes-Ressource [Eingang](https://kubernetes.io/docs/user-guide/ingress/). Ein Eingang ist eine Sammlung von Regeln, die eingehenden Verbindungen das Aufrufen der Clusterdienste erlauben. Damit eine Eingangsressource funktioniert, muss in Kubernetes-Clustern ein [Eingangscontroller](https://kubernetes.io/docs/user-guide/ingress/#ingress-controllers) ausgeführt werden.

Azure Container Service implementiert einen Kubernetes-Eingangscontroller nicht automatisch. Mehrere Controllerimplementierungen sind verfügbar. Derzeit wird für [Nginx Ingress controller](https://github.com/kubernetes/ingress/tree/master/examples/deployment/nginx) (Nginx-Eingangscontroller) empfohlen, Eingangsregeln zu konfigurieren und einen Lastenausgleich für HTTP- und HTTPS-Datenverkehr vorzunehmen. 

Weitere Informationen finden Sie in der [Nginx Ingress controller documentation](https://github.com/kubernetes/ingress/tree/master/controllers/nginx/README.md) (Dokumentation zum Nginx-Eingangscontroller).

> [!IMPORTANT]
> Bei Verwendung des Nginx-Eingangscontrollers in Azure Container Service müssen Sie die Controller-Bereitstellung als Dienst mit `type: LoadBalancer` zur Verfügung stellen. Dadurch wird Azure Load Balancer zum Weiterleiten von Datenverkehr an den Controller konfiguriert. Weitere Informationen finden Sie im vorherigen Abschnitt.


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen finden Sie in der [Kubernetes LoadBalancer documentation](https://kubernetes.io/docs/user-guide/load-balancer/) (Dokumentation zum Kubernetes-Lastenausgleich).
* Weitere Informationen über [Kubernetes Ingress and Ingress controllers](https://kubernetes.io/docs/user-guide/ingress/) (Eingang und Eingangscontroller in Kubernetes)
* Weitere Informationen finden Sie unter [Kubernetes examples](https://github.com/kubernetes/kubernetes/tree/master/examples) (Kubernetes-Beispiele).


