---

title: "OMS-Containerlösung | Microsoft Docs"
description: "Die OMS-Containerlösung ist eine Lösung in OMS Log Analytics, mit der Sie das Inventar, die Leistung und die Protokolle von Containern an einer zentralen Stelle anzeigen können. Sie können Container überwachen und Problembehebungen durchführen, indem Sie die Protokolle an einer zentralen Stelle anzeigen und Container mit hoher Aktivität auf einem Host suchen."
services: container-service
documentationcenter: 
author: keikhara
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: 
ms.assetid: 01f1a77e-f3e1-4ec0-ad4c-d91298afa55c
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 11/17/2016
ms.author: keikhara
translationtype: Human Translation
ms.sourcegitcommit: 10770c96900737e1894405a6aedaa7fe2a55fd3d
ms.openlocfilehash: 9697a4c23b50b5ed2435a00a31fa2ee239c56c48


---

# <a name="using-oms-to-monitor-container-applications-on-acs-dcos"></a>Verwenden von OMS zum Überwachen von Containeranwendungen in ACS DC/OS

Microsoft Operations Management (OMS) ist die cloudbasierte IT-Verwaltungslösung von Microsoft, die Sie beim Verwalten und Schützen Ihrer Infrastruktur lokal und in der Cloud unterstützt. Die Containerlösung ist eine Lösung in OMS Log Analytics, mit der Sie das Inventar, die Leistung und die Protokolle von Containern an einer zentralen Stelle anzeigen können. Sie können Container überwachen und Problembehebungen durchführen, indem Sie die Protokolle an einer zentralen Stelle anzeigen und Container mit zu hoher Aktivität und zu hohem Ressourcenverbrauch auf einem Host suchen.

![](media/container-service-monitoring-oms/image1.png)

Weitere Informationen zur Containerlösung finden Sie unter [Log Analytics-Containerlösung](../log-analytics/log-analytics-containers.md).

## <a name="setting-up-oms-from-the-dcos-universe"></a>Einrichten von OMS in DC/OS-Umgebungen


In diesem Artikel wird vorausgesetzt, dass Sie DC/OS eingerichtet und einfache Webcontaineranwendungen im Cluster bereitgestellt haben.

### <a name="pre-requisite"></a>Voraussetzung
- [Microsoft Azure-Abonnement](https://azure.microsoft.com/free/) – hierfür steht Ihnen eine kostenlose Testversion zur Verfügung.  
- Einrichtung des Microsoft OMS-Arbeitsbereichs – siehe „Schritt 3“ weiter unten.
- [DC/OS-Befehlszeilenschnittstelle](https://dcos.io/docs/1.8/usage/cli/install/) installiert.

1. Klicken Sie auf dem DC/OS-Dashboard auf „Universe“, und suchen Sie nach „OMS“ – wie unten dargestellt.

![](media/container-service-monitoring-oms/image2.png)

2. Klicken Sie auf **Installieren**. Es wird ein Popupfenster mit den OMS-Versionen sowie die Schaltflächen **Install Package** oder **Advanced Installation** angezeigt. Wenn Sie auf **Advanced Installation** klicken, werden Sie zur Seite **OMS specific configuration properties** weitergeleitet.

![](media/container-service-monitoring-oms/image3.png)

![](media/container-service-monitoring-oms/image4.png)

3. Hier werden Sie dazu aufgefordert, die Werte für `wsid` (Arbeitsbereichs-ID für OMS) und `wskey` (der primäre OMS-Schlüssel für die Arbeitsbereichs-ID) einzugeben. Um `wsid` und `wskey` zu erhalten, müssen Sie ein OMS-Konto unter <https://mms.microsoft.com> erstellen.
Führen Sie die entsprechenden Schritte aus, um ein Konto zu erstellen. Nachdem Sie das Konto erstellt haben, müssen Sie Ihre `wsid` und `wskey` abrufen, indem Sie auf **Einstellungen**, **Verbundene Quellen** und **Linux-Server** klicken, wie unten dargestellt.

 ![](media/container-service-monitoring-oms/image5.png)

4. Wählen Sie die Anzahl der gewünschten OMS-Instanzen aus, und klicken Sie auf die Schaltfläche „Überprüfen und Installieren“. In der Regel sollte die Anzahl von OMS-Instanzen der Anzahl von virtuellen Computern in Ihrem Agent-Cluster entsprechen. Der OMS-Agent für Linux ist auf jedem virtuellen Computer, für den Überwachungs- und Anmeldeinformationen gesammelt werden sollen, als individueller Container installiert.

## <a name="setting-up-a-simple-oms-dashboard"></a>Einrichten eines einfachen OMS-Dashboards

Nachdem Sie den OMS-Agent für Linux auf den virtuellen Computern installiert haben, richten Sie im nächsten Schritt das OMS-Dashboard ein. Dafür gibt es zwei Möglichkeiten: über das OMS-oder das Azure-Portal.

### <a name="oms-portal"></a>OMS-Portal 

Melden Sie sich beim OMS-Portal an (<https://mms.microsoft.com>), und wechseln Sie zum **Lösungskatalog**.

![](media/container-service-monitoring-oms/image6.png)

Wählen Sie im **Lösungskatalog** die Option **Container**.

![](media/container-service-monitoring-oms/image7.png)

Nachdem Sie die Containerlösung ausgewählt haben, wird die Kachel auf dem Übersichts-Dashboard für OMS angezeigt. Sobald die erfassten Containerdaten indiziert sind, werden die Kacheln zur Ansicht der Lösung mit Informationen aufgefüllt.

![](media/container-service-monitoring-oms/image8.png)

### <a name="azure-portal"></a>Azure-Portal 

Melden Sie sich unter <https://portal.microsoft.com/> beim Azure-Portal an. Wechseln Sie zu **Marketplace**, wählen Sie **Überwachung und Verwaltung**, und klicken Sie auf **Alle anzeigen**. Geben Sie „`containers`container“ in das Suchfeld ein. Der Begriff „Container“ wird in den Suchergebnissen angezeigt. Wählen Sie **Container**, und klicken Sie auf **Erstellen**.

![](media/container-service-monitoring-oms/image9.png)

Nachdem Sie auf **Erstellen** geklickt haben, werden Sie nach Ihrem Arbeitsbereich gefragt. Wählen Sie Ihren Arbeitsbereich aus, oder erstellen Sie einen neuen, wenn Sie noch keinen besitzen.

![](media/container-service-monitoring-oms/image10.PNG)

Nach dem Auswählen Ihres Arbeitsbereichs klicken Sie auf **Erstellen**.

![](media/container-service-monitoring-oms/image11.png)

Weitere Informationen zur OMS-Containerlösung finden Sie unter [Log Analytics-Containerlösung](../log-analytics/log-analytics-containers.md).

### <a name="how-to-scale-oms-agent-with-acs-dcos"></a>Skalieren des OMS-Agents mit ACS DC/OS 

Wenn der OMS-Agent nicht auf der richtigen Anzahl von Knoten bereitgestellt wurde oder wenn Sie VMSS durch Hinzufügen weiterer virtueller Computer hochskalieren möchten, können Sie dies durch Skalieren des `msoms`-Diensts erreichen.

Sie können dazu entweder zur Registerkarte „Marathon“ oder „DC/OS UI-Dienste“ wechseln und Ihre Knotenanzahl zentral hochskalieren.

![](media/container-service-monitoring-oms/image12.PNG)

Dadurch erfolgt die Bereitstellung auf anderen Knoten, auf denen der OMS-Agent noch nicht bereitgestellt wurde.

## <a name="uninstall-ms-oms"></a>Deinstallieren der MS OMS

Geben Sie den folgenden Befehl ein, um die MS OMS zu deinstallieren:

```bash
$ dcos package uninstall msoms
```

## <a name="let-us-know"></a>Informieren Sie uns darüber!
Was funktioniert? Was fehlt? Was benötigen Sie noch, was nützlich für Sie sein könnte? Informieren Sie uns: <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.

## <a name="next-steps"></a>Nächste Schritte

 Nachdem Sie OMS für die Überwachung Ihrer Container eingerichtet haben, [sehen Sie sich Ihr Container-Dashboard an](../log-analytics/log-analytics-containers.md).



<!--HONumber=Nov16_HO3-->


