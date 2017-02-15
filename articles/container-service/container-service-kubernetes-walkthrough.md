---
title: Bereitstellen eines Azure Container Service-Clusters mit Kubernetes | Microsoft Docs
description: Bereitstellen eines Azure Container Service-Clusters mit Kubernetes
services: container-service
documentationcenter: 
author: anhowe
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2016
ms.author: anhowe
translationtype: Human Translation
ms.sourcegitcommit: 0989c85e4b7c73a8b7b335254e4af90ef34ddc01
ms.openlocfilehash: 8c10da9d2363f0938d821b59cc4fd58b53f9ed17


---

# <a name="microsoft-azure-container-service-engine---kubernetes-walkthrough"></a>Microsoft Azure Container Service-Modul – Exemplarische Vorgehensweise für Kubernetes

## <a name="deployment"></a>Bereitstellung

Dies sind die Schritte zum Bereitstellen eines einfachen Kubernetes-Clusters:

1. [Generate your ssh key](https://github.com/Azure/azure-quickstart-templates/blob/master/101-acs-dcos/docs/SSHKeyManagement.md#ssh-key-generation) (Generieren des SSH-Schlüssels)
3. [Generate your service principal](https://github.com/Azure/acs-engine/blob/master/docs/serviceprincipal.md) (Generieren des Dienstprinzipals)
4. [Klicken Sie in der README-Datei auf die Schaltfläche „Deploy to Azure“](https://github.com/Azure/azure-quickstart-templates/blob/master/101-acs-kubernetes/README.md), und füllen Sie die Felder aus.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Nachdem Ihr Kubernetes-Cluster erstellt wurde, verfügen Sie über eine Ressourcengruppe mit folgendem Inhalt:

1. 1 Master, der per SSH über Port 22 oder kubectl über Port 443 zugänglich ist

2. Gruppe von Knoten in einer Verfügbarkeitsgruppe  Auf die Knoten kann über einen Master zugegriffen werden.  Ein Beispiel zur Vorgehensweise finden Sie unter [Agent forwarding](https://github.com/Azure/azure-quickstart-templates/blob/master/101-acs-dcos/docs/SSHKeyManagement.md#key-management-and-agent-forwarding-with-windows-pageant) (Agent-Weiterleitung).

Die folgende Abbildung zeigt die Architektur eines Container Service-Clusters mit einem Master und zwei Agents:

![Abbildung: Kubernetes-Cluster in Azure](media/container-service-kubernetes-walkthrough/kubernetes.png)

In der obigen Abbildung sind die folgenden Bestandteile dargestellt:

1. **Masterkomponenten**: Der Master führt den Kubernetes-Scheduler, API-Server und Controller-Manager aus.  Port 443 wird für die Remoteverwaltung mit der kubectl-CLI verfügbar gemacht.
2. **Knoten**: Die Kubernetes-Knoten werden in einer Verfügbarkeitsgruppe ausgeführt.  Azure Load Balancers werden dem Cluster je nach den verfügbar gemachten Diensten hinzugefügt. 
3. **Allgemeine Komponenten**: Auf allen VMs werden ein Kubelet, Docker und ein Proxy ausgeführt.
4. **Netzwerk**: Allen VMs wird eine IP-Adresse im Netzwerk 10.240.0.0/16 hinzugefügt.  Jeder VM wird ein /24-Subnetz pro Pod für die IP zur Pod-CIDR-Aktivierung hinzugefügt.  Mit dem Proxy, der auf jeder VM ausgeführt wird, wird das Dienstnetzwerk 10.0.0.0/16 implementiert.

Alle VMs befinden sich in demselben privaten VNET, und untereinander besteht Vollzugriff.

## <a name="create-your-first-kubernetes-service"></a>Erstellen des ersten Kubernetes-Diensts

Nach Abschluss dieser exemplarischen Vorgehensweise besitzen Sie folgende Kenntnisse:
 * Zugreifen auf den Kubernetes-Cluster per SSH
 * Bereitstellen und globales Verfügbarmachen einer einfachen Docker-Anwendung
 * Ermitteln der Kube-Konfigurationsdatei und Zugreifen auf den Kubernetes-Cluster von einem Remotestandort
 * Verwenden von `kubectl exec` zum Ausführen von Befehlen in einem Container 
 * Zugreifen auf das Kubernetes-Dashboard

1. Notieren Sie sich nach der erfolgreichen Bereitstellung der Vorlage die Master-FQDNs (vollqualifizierten Domänennamen).
   1. Bei Verwendung von PowerShell oder der CLI befindet sich der Ausgabeparameter im OutputsString-Abschnitt mit dem Namen „masterFQDN“.
   2. Navigieren Sie bei Verwendung des Portals zum Blatt „Übersicht“ der ContainerService-Ressource, um den „Master-FQDN“ zu kopieren:
     
   ![Abbildung: Docker-Skalierung](media/container-service-kubernetes-walkthrough/portal-kubernetes-outputs.png)

2. Stellen Sie eine SSH-Verbindung mit dem Master-FQDN aus Schritt 1 her.

3. Prüfen Sie Ihre Knoten und ausgeführten Pods:
  1. Geben Sie Folgendes ein, um eine Liste mit Ihren Knotentypen anzuzeigen: `kubectl get nodes`.  Wenn Sie alle Details der Knoten anzeigen möchten, können Sie `-o yaml` hinzufügen: `kubectl get nodes -o yaml`.
  2. Geben Sie Folgendes ein, um eine Liste mit den ausgeführten Pods zu erhalten: `kubectl get pods --all-namespaces`.

4. Starten Sie Ihr erstes Docker-Image, indem Sie Folgendes eingeben: `kubectl run nginx --image nginx`.  Der nginx-Docker-Container wird in einem Pod auf einem der Knoten gestartet.

5. Geben Sie `kubectl get pods -o yaml` ein, um alle Details der nginx-Bereitstellung anzuzeigen. Sie sehen die Host-IP und die Pod-IP.  Die Pod-IP wird über Pod-CIDR auf dem Host zugewiesen.  Führen Sie Curl für die Pod-IP aus, um die nginx-Ausgabe anzuzeigen, z.B.: `curl 10.244.1.4`

  ![Abbildung: Curl für podIP](media/container-service-kubernetes-walkthrough/kubernetes-nginx1.png)

6. Der nächste Schritt ist das Verfügbarmachen der nginx-Bereitstellung als Kubernetes-Dienst im privaten Dienstnetzwerk 10.0.0.0/16:
  1. Machen Sie den Dienst mit dem Befehl `kubectl expose deployment nginx --port=80` verfügbar.
  2. Rufen Sie die Dienst-IP ab: `kubectl get service`.
  3. Führen Sie Curl für die IP aus, z.B.: `curl 10.0.105.199`

  ![Abbildung: Curl für Dienst-IP](media/container-service-kubernetes-walkthrough/kubernetes-nginx2.png)

7. Der letzte Schritt ist das globale Verfügbarmachen des Diensts.  Ändern Sie hierzu den Diensttyp von `ClusterIP` in `LoadBalancer`:
  1. Bearbeiten Sie den Dienst: `kubectl edit svc/nginx`
  2. Ändern Sie `type` von `ClusterIP` in `LoadBalancer`, und speichern Sie.  Dies bewirkt, dass Kubernetes einen Azure Load Balancer mit einer öffentlichen IP erstellt.
  3. Diese Änderung nimmt etwa 2 bis 3 Minuten in Anspruch.  Geben Sie `watch 'kubectl get svc'` ein, um zu verfolgen, wie sich der Dienst von „pending“ (Ausstehend) in eine externe IP ändert.

  ![Abbildung: Verfolgen des Übergangs von „pending“ zur externen IP](media/container-service-kubernetes-walkthrough/kubernetes-nginx3.png)

  4. Wenn die externe IP angezeigt wird, können Sie im Browser dorthin navigieren:

  ![Abbildung: Navigieren zu nginx](media/container-service-kubernetes-walkthrough/kubernetes-nginx4.png)  

8. Im nächsten Schritt dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Sie die Remoteverwaltung für Ihren Kubernetes-Cluster durchführen.  Laden Sie zunächst Kubectl auf Ihren Computer herunter, und fügen Sie die Anwendung unter dem entsprechenden Pfad ein:
  * [Windows Kubectl](https://storage.googleapis.com/kubernetes-release/release/v1.4.5/bin/windows/amd64/kubectl.exe)
  * [OSX Kubectl](https://storage.googleapis.com/kubernetes-release/release/v1.4.5/bin/darwin/amd64/kubectl)
  * [Linux](https://storage.googleapis.com/kubernetes-release/release/v1.4.5/bin/linux/amd64/kubectl)

9. Der Kubernetes-Master enthält die kube-Konfigurationsdatei für den Remotezugriff im Stammverzeichnis „~/.kube/config“.  Laden Sie diese Datei auf Ihren Computer herunter, legen Sie die KUBECONFIG-Umgebungsvariable fest, und führen Sie kubectl aus, um sicherzustellen, dass Sie eine Verbindung mit dem Cluster herstellen können:
  * Für Windows wird pscp von [putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) verwendet.  Stellen Sie sicher, dass Sie Ihr Zertifikat über [pageant](https://github.com/Azure/acs-engine/blob/master/docs/ssh.md#key-management-and-agent-forwarding-with-windows-pageant) verfügbar gemacht haben:
  ```
  # MASTERFQDN is obtained in step1
  pscp azureuser@MASTERFQDN:.kube/config .
  SET KUBECONFIG=%CD%\config
  kubectl get nodes
  ```
  * OS X oder Linux:
  ```
  # MASTERFQDN is obtained in step1
  scp azureuser@MASTERFQDN:.kube/config .
  export KUBECONFIG=`pwd`/config
  kubectl get nodes
  ```
10. Im nächsten Schritt wird gezeigt, wie Sie die Remoteausführung von Befehlen in einem Docker-Remotecontainer durchführen:
  1. Führen Sie `kubectl get pods` aus, um den Namen Ihres nginx-Pods anzuzeigen.
  2. Mit dem Pod-Namen können Sie einen Remotebefehl auf dem Pod ausführen.  Beispiel: `kubectl exec nginx-701339712-retbj date`
  3. Versuchen Sie, eine Bash-Remotesitzung auszuführen. Beispiel: `kubectl exec nginx-701339712-retbj -it bash`.  Im folgenden Screenshot sind diese Befehle dargestellt:

  ![Abbildung: Curl für podIP](media/container-service-kubernetes-walkthrough/kubernetes-remote.png)

11. Im letzten Schritt dieses Tutorials erhalten Sie einen Einblick in das Dashboard:
  1. Führen Sie `kubectl proxy` aus, um eine direkte Verbindung mit dem Proxy herzustellen.
  2. Navigieren Sie in Ihrem Browser zum [Dashboard](http://127.0.0.1:8001/api/v1/proxy/namespaces/kube-system/services/kubernetes-dashboard/#/workload?namespace=_all).
  3. Sehen Sie sich die Details Ihrer Pods und Dienste an.
  ![Abbildung: Kubernetes-Dashboard](media/container-service-kubernetes-walkthrough/kubernetes-dashboard.png)

# <a name="learning-more"></a>Weitere Informationen

Unter diesen empfohlenen Links finden Sie weitere Informationen zu Kubernetes:

1. [Dokumentation zu Azure Container Service](https://azure.microsoft.com/en-us/documentation/services/container-service/)

## <a name="kubernetes-community-documentation"></a>Dokumentation zur Kubernetes-Community

1. [Kubernetes Bootcamp](https://kubernetesbootcamp.github.io/kubernetes-bootcamp/index.html): Es wird veranschaulicht, wie Sie Anwendungen in Containern bereitstellen, skalieren, aktualisieren und debuggen.
2. [Kubernetes Userguide](http://kubernetes.io/docs/user-guide/) (Kubernetes-Benutzerhandbuch): Enthält Informationen zum Ausführen von Programmen in einem vorhandenen Kubernetes-Cluster.
3. [Kubernetes Examples](https://github.com/kubernetes/kubernetes/tree/master/examples) (Kubernetes-Beispiele): Enthält Beispiele für die Ausführung von echten Anwendungen mit Kubernetes.



<!--HONumber=Nov16_HO3-->


