<properties
   pageTitle="Azure Container Service-Containerverwaltung über die Web-Benutzeroberfläche | Microsoft Azure"
   description="Stellen Sie mithilfe der Marathon-Webbenutzeroberfläche Container für einen Azure Container Service-Clusterdienst bereit."
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Container, Microservices, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/16/2016"
   ms.author="nepeters"/>

# Containerverwaltung über die Webbenutzeroberfläche

DC/OS stellt eine Umgebung für die Bereitstellung und Skalierung geclusterter Workloads bereit und abstrahiert die zugrunde liegende Hardware. Zusätzlich zu DC/OS ist auch ein Framework vorhanden, mit dem die Planung und Ausführung von Computeworkloads verwaltet wird.

Es sind zwar Frameworks für viele gängige Workloads verfügbar, aber in diesem Dokument wird beschrieben, wie Sie Containerbereitstellungen mit Marathon erstellen und skalieren. Bevor Sie diese Beispiele durcharbeiten, benötigen Sie einen DC/OS-Cluster, der im Azure Container Service konfiguriert ist. Sie müssen auch über Remoteverbindungen mit diesem Cluster verfügen. Weitere Informationen zu diesen Elementen finden Sie in den folgenden Artikeln:

- [Bereitstellen eines Azure Container Service-Clusters](./container-service-deployment.md)
- [Verbinden mit einem Azure Container Service-Cluster](./container-service-connect.md)

## Untersuchen der DC/OS-Benutzeroberfläche

Navigieren Sie mit eingerichtetem Secure Shell (SSH)-Tunnel zu http://localhost/. Dadurch wird die DC/OS-Webbenutzeroberfläche geladen, und es werden Informationen zum Cluster angezeigt, etwa genutzte Ressourcen, aktive Agents und ausgeführte Dienste.

![](media/dcos/dcos2.png)

## Erkunden der Marathon-Benutzeroberfläche

Navigieren Sie zum Anzeigen der Marathon-Benutzeroberfläche zu http://localhost/Marathon. Auf diesem Bildschirm können Sie einen neuen Container oder eine andere Anwendung auf dem Azure Container Service-DC/OS-Cluster starten. Sie können auch Informationen zu ausgeführten Containern und Anwendungen anzeigen.

![](media/dcos/dcos3.png)

## Bereitstellen eines Containers im Docker-Format

Klicken Sie zum Bereitstellen eines neuen Containers mithilfe von Marathon auf die Schaltfläche **Anwendung erstellen**, und geben Sie die folgenden Informationen in das Formular ein. Klicken Sie anschließend auf **Anwendung erstellen**.

Feld | Wert
----------------|-----------
ID | nginx
Image | nginx
Netzwerk | Überbrückt
Hostport | 80
Protokoll | TCP

![](media/dcos/dcos4.png)

![](media/dcos/dcos5.png)

![](media/dcos/dcos6.png)

Verwenden Sie zum statischen Zuordnen eines Containerports zu einem Port des Agents den JSON-Modus. Wechseln Sie im Assistenten für die neue Anwendung mithilfe des Umschalters in den JSON-Modus, und geben Sie im Abschnitt „portMappings“ der Anwendungsdefinition Folgendes ein. In diesem Beispiel wird Port 80 des Containers an Port 80 des DC/OS-Agents gebunden. Sobald die Änderung vorgenommen wurde, kann der JSON-Modus im Assistenten beendet werden.

```none
“hostPort”: 80,
```

![](media/dcos/dcos13.png)

Der DC/OS-Cluster wird mit einem Satz von privaten und öffentlichen Agents bereitgestellt. Damit über das Internet auf Anwendungen zugegriffen werden kann, müssen die Anwendungen auf einem öffentlichen Agent bereitgestellt werden. Wählen Sie dazu im Assistenten für die neue Anwendung die Registerkarte „Optional“, und geben Sie für die akzeptierten Ressourcenrollen „slave\_public“ ein.

![](media/dcos/dcos14.png)

Auf der Marathon-Hauptseite können Sie den Bereitstellungsstatus für den Container anzeigen.

![](media/dcos/dcos7.png)

Wenn Sie zurück zur DC/OS-App wechseln (http://localhost/), sehen Sie, dass jetzt eine Aufgabe – in diesem Fall ein Container im Docker-Format – auf dem DC/OS-Cluster ausgeführt wird.

![](media/dcos/dcos8.png)

Sie sehen auch, auf welchem Clusterknoten die Aufgabe ausgeführt wird.

![](media/dcos/dcos9.png)

## Skalieren der Container

Auf der Marathon-Benutzeroberfläche kann die Anzahl der Instanzen eines Containers skaliert werden. Navigieren Sie dazu zur Marathon-Seite, wählen Sie den zu skalierenden Container aus, und klicken Sie auf die Schaltfläche **Skalieren**. Geben Sie im Fenster **Anwendung skalieren** die gewünschte Anzahl der Containerinstanzen ein, und wählen Sie **Anwendung skalieren**.

![](media/dcos/dcos10.png)

Sobald der Skalierungsvorgang abgeschlossen ist, sehen Sie mehrere Instanzen derselben Aufgabe auf DC/OS-Agents verteilt.

![](media/dcos/dcos11.png)

![](media/dcos/dcos12.png)

## Nächste Schritte

[Arbeiten mit der DC/OS- und Marathon-API](./container-service-mesos-marathon-rest.md)

<!---HONumber=AcomDC_0427_2016-->