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

Mesos stellt eine Umgebung für die Bereitstellung und Skalierung geclusterter Workloads bereit und abstrahiert die zugrunde liegende Hardware. Zusätzlich zu Mesos ist auch ein Framework vorhanden, mit dem die Planung und Ausführung von Computeworkloads verwaltet wird.

Es sind zwar Frameworks für viele gängige Workloads verfügbar, aber in diesem Dokument wird beschrieben, wie Sie Containerbereitstellungen mit Marathon erstellen und skalieren. Bevor Sie diese Beispiele durcharbeiten, benötigen Sie einen Mesos-Cluster, der im Azure Container Service konfiguriert ist. Sie müssen auch über Remoteverbindungen mit diesem Cluster verfügen. Weitere Informationen zu diesen Elementen finden Sie in den folgenden Artikeln:

- [Bereitstellen eines Azure Container Service-Clusters](./container-service-deployment.md)
- [Verbinden mit einem Azure Container Service-Cluster](./container-service-connect.md)

## Erkunden der Mesos-Benutzeroberfläche

Navigieren Sie mit eingerichtetem Secure Shell (SSH)-Tunnel zu http://localhost/Mesos. Dadurch wird die Mesos-Webbenutzeroberfläche geladen. Auf der Seite können Sie Informationen zum Mesos-Cluster erfassen, z.B. aktivierte Agents, Aufgabenstatus und die Ressourcenverfügbarkeit.

![Bereitstellung erstellen, UI 1](media/ui1.png)

## Erkunden der Marathon-Benutzeroberfläche

Navigieren Sie zum Anzeigen der Marathon-Benutzeroberfläche zu http://localhost/Marathon. Auf diesem Bildschirm können Sie einen neuen Container oder eine andere Anwendung auf dem Azure Container Service-Mesos-Cluster starten. Sie können auch Informationen zu ausgeführten Containern und Anwendungen anzeigen.

![Bereitstellung erstellen, UI 2](media/ui2.png)

## Bereitstellen eines Containers im Docker-Format

Um mit Marathon einen neuen Container auf dem Mesos-Cluster zu starten, klicken Sie auf die Schaltfläche **.Anwendung erstellen**. Verwenden Sie das Formular **Neue Anwendung** zum Definieren der Anwendungs- oder Containerparameter. In diesem Beispiel stellen Sie einen einfachen nginx-Container bereit. Geben Sie Folgendes ein: Wenn Sie fertig sind, klicken Sie auf **Weiter**.

Feld | Wert
----------------|-----------
ID | nginx
Image | nginx
Netzwerk | Überbrückt
Container-Port | 80
Hostport | 80
Protokoll | TCP

![Bereitstellung erstellen, UI 3](media/ui3.png)

Auf der Marathon-Hauptseite können Sie den Bereitstellungsstatus für den Container anzeigen.

![Bereitstellung erstellen, UI 4](media/ui4.png)

Wenn Sie zurück zur Mesos-App wechseln (http://localhost/Mesos), sehen Sie, dass jetzt eine Aufgabe – in diesem Fall ein Container im Docker-Format – auf dem Mesos-Cluster ausgeführt wird. Sie sehen auch, auf welchem Clusterknoten die Aufgabe ausgeführt wird.

![Bereitstellung erstellen, UI 5](media/ui5.png)

## Skalieren der Container

Sie können auf der Marathon-Webbenutzeroberfläche auch die Anzahl der Instanzen eines Containers skalieren. Navigieren Sie dazu zur Marathon-Seite, wählen Sie den zu skalierenden Container aus, und klicken Sie auf die Schaltfläche **Skalieren**. Geben Sie im Fenster **Anwendung skalieren** die gewünschte Anzahl der Containerinstanzen ein, und wählen Sie **Anwendung skalieren**.

![Bereitstellung erstellen, UI 6](media/ui6.png)

Sobald der Skalierungsvorgang abgeschlossen ist, sehen Sie mehrere Instanzen derselben Aufgabe auf Mesos-Agents verteilt.

![Bereitstellung erstellen, UI 7](media/ui8.png)

<!---HONumber=AcomDC_0406_2016-->