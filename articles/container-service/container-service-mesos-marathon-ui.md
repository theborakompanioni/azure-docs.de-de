---
title: "Verwalten eines Azure-DC/OS-Clusters mit Marathon-Benutzeroberfläche | Microsoft-Dokumentation"
description: "Stellen Sie mit der Marathon-Webbenutzeroberfläche Container für einen Azure Container Service-Clusterdienst bereit."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Container, Microservices, Mesos, Azure
ms.assetid: d148ed1e-b582-4d51-944f-1ac7ae3c4fd6
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/02/2016
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: 0aa9b3ae14f586fc79e6ebee898e794d526c19bd
ms.openlocfilehash: de250fbc3982cdc493f74976914290d8ac89bb08


---
# <a name="manage-an-azure-container-service-dcos-cluster-through-the-marathon-web-ui"></a>Verwalten eines Azure Container Service-DC/OS-Clusters über die Marathon-Webbenutzeroberfläche
DC/OS stellt eine Umgebung für die Bereitstellung und Skalierung geclusterter Workloads bereit und abstrahiert die zugrunde liegende Hardware. Zusätzlich zu DC/OS ist auch ein Framework vorhanden, mit dem die Planung und Ausführung von Computeworkloads verwaltet wird.

Es sind zwar Frameworks für viele gängige Workloads verfügbar, in diesem Dokument wird jedoch beschrieben, wie Sie Containerbereitstellungen mit Marathon erstellen und skalieren. 


## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie diese Beispiele durcharbeiten, benötigen Sie einen DC/OS-Cluster, der im Azure Container Service konfiguriert ist. Sie müssen auch über Remoteverbindungen mit diesem Cluster verfügen. Weitere Informationen zu diesen Elementen finden Sie in den folgenden Artikeln:

* [Bereitstellen eines Azure Container Service-Clusters](container-service-deployment.md)
* [Verbinden mit einem Azure Container Service-Cluster](container-service-connect.md)

## <a name="explore-the-dcos-ui"></a>Untersuchen der DC/OS-Benutzeroberfläche
Navigieren Sie [nach der Einrichtung](container-service-connect.md) eines SSH-Tunnels (Secure Shell) zu „http://localhost/“. Dadurch wird die DC/OS-Webbenutzeroberfläche geladen, und es werden Informationen zum Cluster angezeigt, etwa genutzte Ressourcen, aktive Agents und ausgeführte Dienste.

![DC/OS-Benutzeroberfläche](./media/dcos/dcos2.png)

## <a name="explore-the-marathon-ui"></a>Erkunden der Marathon-Benutzeroberfläche
Um die Marathon-Benutzeroberfläche anzuzeigen, navigieren Sie zu „http://localhost/Marathon“. Auf diesem Bildschirm können Sie einen neuen Container oder eine andere Anwendung im Azure Container Service-DC/OS-Cluster starten. Sie können auch Informationen zu ausgeführten Containern und Anwendungen anzeigen.  

![Marathon-Benutzeroberfläche](./media/dcos/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a>Bereitstellen eines Containers im Docker-Format
Klicken Sie zum Bereitstellen eines neuen Containers mit Marathon auf **Anwendung erstellen**, und geben Sie die folgenden Informationen in die Formularregisterkarten ein:

| Feld | Wert |
| --- | --- |
| ID |nginx |
| Arbeitsspeicher | 32 |
| Image |nginx |
| Netzwerk |Überbrückt |
| Hostport |80 |
| Protocol |TCP |

![Neue Anwendungsbenutzeroberfläche – Allgemein](./media/dcos/dcos4.png)

![Neue Anwendungsbenutzeroberfläche – Docker-Container](./media/dcos/dcos5.png)

![Neue Anwendungsbenutzeroberfläche – Ports und Dienstermittlung](./media/dcos/dcos6.png)

Sie müssen den JSON-Modus verwenden, wenn Sie den Containerport statisch einem Port auf dem Agent zuordnen möchten. Wechseln Sie im Assistenten „Neue Anwendung“ hierzu mit dem Umschalter in den **JSON-Modus** . Geben Sie anschließend im Abschnitt `portMappings` der Anwendungsdefinition die folgende Einstellung ein. In diesem Beispiel wird Port 80 des Containers an Port 80 des DC/OS-Agents gebunden. Nachdem Sie diese Änderung vorgenommen haben, können Sie den JSON-Modus für den Assistenten wieder deaktivieren.

```none
"hostPort": 80,
```

![Neue Anwendungsbenutzeroberfläche – Beispiel für Port 80](./media/dcos/dcos13.png)

Falls Sie Integritätsprüfungen aktivieren möchten, legen Sie auf der Registerkarte **Integritätsprüfungen** einen Pfad fest.

![Neue Anwendungsbenutzeroberfläche – Integritätsprüfungen](./media/dcos/dcos_healthcheck.png)

Der DC/OS-Cluster wird mit einem Satz von privaten und öffentlichen Agents bereitgestellt. Damit der Cluster auf Anwendungen über das Internet zugreifen kann, müssen Sie die Anwendungen auf einem öffentlichen Agent bereitstellen. Wählen Sie dazu im Assistenten für die neue Anwendung die Registerkarte **Optional**, und geben Sie unter **Accepted Resource Roles** (Akzeptierte Ressourcenrollen) den Text **slave_public** ein.

Klicken Sie dann auf **Anwendung erstellen**.

![Neue Anwendungsbenutzeroberfläche – Einstellung für öffentlichen Agent](./media/dcos/dcos14.png)

Auf der Marathon-Hauptseite können Sie den Bereitstellungsstatus für den Container anzeigen. Zu Beginn wird der Status **Wird bereitgestellt...** angezeigt. Nach erfolgreicher Bereitstellung ändert sich der Status in **Wird ausgeführt...**.

![Hauptseite der Marathon-Benutzeroberfläche – Containerbereitstellungsstatus](./media/dcos/dcos7.png)

Wenn Sie wieder zur DC/OS-Webbenutzeroberfläche (http://localhost/) wechseln, sehen Sie, dass eine Aufgabe – in diesem Fall ein Container im Docker-Format – im DC/OS-Cluster ausgeführt wird.

![DC/OS-Webbenutzeroberfläche – Im Cluster ausgeführte Aufgabe](./media/dcos/dcos8.png)

Wenn Sie den Clusterknoten ermitteln möchten, auf dem die Aufgabe ausgeführt wird, klicken Sie auf die Registerkarte **Knoten**.

![DC/OS-Webbenutzeroberfläche – Aufgabenclusterknoten](./media/dcos/dcos9.png)

## <a name="scale-your-containers"></a>Skalieren der Container
Sie können auf der Marathon-Benutzeroberfläche die Anzahl der Instanzen eines Containers skalieren. Navigieren Sie hierfür zur Seite **Marathon**, wählen Sie den zu skalierenden Container aus, und klicken Sie auf **Anwendung skalieren**. Geben Sie im Dialogfeld **Anwendung skalieren** die gewünschte Anzahl von Containerinstanzen ein, und klicken Sie anschließend auf **Anwendung skalieren**.

![Marathon-Benutzeroberfläche – Dialogfeld „Anwendung skalieren“](./media/dcos/dcos10.png)

Sobald der Skalierungsvorgang abgeschlossen ist, sehen Sie, dass mehrere Instanzen derselben Aufgabe auf DC/OS-Agents verteilt sind.

![Dashboard der DC/OS-Webbenutzeroberfläche – Auf Agents verteilte Aufgabe](./media/dcos/dcos11.png)

![DC/OS-Webbenutzeroberfläche – Knoten](./media/dcos/dcos12.png)

## <a name="next-steps"></a>Nächste Schritte
* [Verwenden von DC/OS und der Marathon-API](container-service-mesos-marathon-rest.md)

* Deep dive on the Azure Container Service with Mesos (Ausführliche Informationen zu Azure Container Service mit Mesos)

    > [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON203/player]
    > 



<!--HONumber=Feb17_HO3-->


