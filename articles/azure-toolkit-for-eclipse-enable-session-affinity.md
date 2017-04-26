---
title: "Aktivieren der Sitzungsaffinität mit dem Azure-Toolkit für Eclipse"
description: "Erfahren Sie mehr über das Aktivieren der Sitzungsaffinität mit dem Azure-Toolkit für Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 88c595ec-7d85-40bd-9078-8d6be7b3f0fa
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: ab8623d6f9751ed6d71d9a5b1c0d5e939c442862
ms.lasthandoff: 04/22/2017


---
# <a name="enable-session-affinity"></a>Aktivieren der Sitzungsaffinität
Im Azure-Toolkit für Eclipse können Sie HTTP-Sitzungsaffinität bzw. „persistente Sitzungen“ für Ihre Rollen aktivieren. Die folgende Abbildung zeigt das Eigenschaftendialogfeld **Lastenausgleich**, anhand dessen das Feature für die Sitzungsaffinität aktiviert werden kann:

![][ic719492]

## <a name="to-enable-session-affinity-for-your-role"></a>So aktivieren Sie die Sitzungsaffinität für Ihre Rolle
1. Klicken Sie im Projekt-Explorer-Bereich von Eclipse auf **Azure** und anschließend auf **Lastenausgleich**.

2. Gehen Sie im Dialogfeld **Properties for WorkerRole1 Load Balancing** wie folgt vor:

   a. Aktivieren Sie **Enable HTTP session affinity (sticky sessions) for this role.**

   b. Wählen Sie für **Input endpoint to use** (Zu verwendender Eingabeendpunkt) den gewünschten Eingabeendpunkt aus, etwa **http (public:80, private:8080)**. Die Anwendung muss diesen Endpunkt als HTTP-Endpunkt verwenden. Sie können mehrere Endpunkte für Ihre Rolle aktivieren, aber nur eine davon für persistente Sitzungen einrichten.

   c. Erstellen Sie Ihre Anwendung neu.

Nach der Aktivierung und bei Vorhandensein mehrerer Rolleninstanzen werden von einem bestimmten Client stammende HTTP-Anforderungen weiterhin von derselben Rolleninstanz behandelt.

Das Eclipse-Toolkit ermöglicht dies durch Installieren eines speziellen IIS-Moduls für den Netzwerklastenausgleich, Application Request Routing (ARR), in jeder der Rolleninstanzen. ARR leitet HTTP-Anforderungen an die entsprechende Rolleninstanz weiter. Das Toolkit konfiguriert automatisch den ausgewählten Endpunkt, damit der eingehende HTTP-Datenverkehr zunächst an die ARR-Software weitergeleitet wird. Das Toolkit erstellt außerdem einen neuen internen Endpunkt, der auf Ihrem Java-Server zum Abhören konfiguriert ist. Dies ist der Endpunkt, der von ARR für das Umleiten des HTTP-Datenverkehrs an die entsprechende Rolleninstanz verwendet wird. Auf diese Weise fungiert jede Rolleninstanz in der Mehrinstanzbereitstellung als Reverseproxy für alle anderen Instanzen, sodass persistente Sitzungen ermöglicht werden.

## <a name="notes-about-session-affinity"></a>Hinweise zur Sitzungsaffinität
* Sitzungsaffinität funktioniert nicht im Serveremulator. Die Einstellungen lassen sich im Serveremulator anwenden, ohne den Erstellungsprozess oder die Ausführung des Serveremulators zu stören, aber die Funktion selbst funktioniert im Serveremulator nicht.

* Das Aktivieren der Sitzungsaffinität führt zu einem erhöhten Speicherplatzbedarf der Bereitstellung in Azure, da zusätzliche Software heruntergeladen und in die Rolleninstanzen installiert wird, wenn der Dienst in der Azure-Cloud gestartet wird.

* Die Dauer der Initialisierung der einzelnen Rollen verlängert sich.

* Es wird ein interner Endpunkt zur Umleitung des Datenverkehrs hinzugefügt, wie oben beschrieben.


## <a name="see-also"></a>Weitere Informationen
[Azure-Toolkit für Eclipse][Azure Toolkit for Eclipse]

[Erstellen einer Hello World-Anwendung für Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]

[Installieren des Azure-Toolkits für Eclipse][Installing the Azure Toolkit for Eclipse] 

Weitere Informationen zum Verwenden von Azure mit Java finden Sie im [Azure Java Developer Center][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[How to Maintain Session Data with Session Affinity]: http://go.microsoft.com/fwlink/?LinkID=699539
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->

