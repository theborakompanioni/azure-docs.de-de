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
ms.date: 11/01/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: a7320a7e5052ffb4171307bc8636182447ddfc50


---
# <a name="enable-session-affinity"></a>Aktivieren der Sitzungsaffinität
Im Azure-Toolkit für Eclipse können Sie HTTP-Sitzungsaffinität bzw. „persistente Sitzungen“ für Ihre Rollen aktivieren. Die folgende Abbildung zeigt das Eigenschaftendialogfeld **Lastenausgleich**, anhand dessen das Feature für die Sitzungsaffinität aktiviert werden kann:

![][ic719492]

## <a name="to-enable-session-affinity-for-your-role"></a>So aktivieren Sie die Sitzungsaffinität für Ihre Rolle
1. Klicken Sie im Projekt-Explorer-Bereich von Eclipse auf **Azure** und anschließend auf **Lastenausgleich**.
2. Gehen Sie im Dialogfeld **Properties for WorkerRole1 Load Balancing** wie folgt vor:
   1. Aktivieren Sie **Enable HTTP session affinity (sticky sessions) for this role.**
   2. Wählen Sie für **Input endpoint to use** (Zu verwendender Eingabeendpunkt) den gewünschten Eingabeendpunkt aus, etwa **http (public:80, private:8080)**. Die Anwendung muss diesen Endpunkt als HTTP-Endpunkt verwenden. Sie können mehrere Endpunkte für Ihre Rolle aktivieren, aber nur eine davon für persistente Sitzungen einrichten.
   3. Erstellen Sie Ihre Anwendung neu.

Nach der Aktivierung und bei Vorhandensein mehrerer Rolleninstanzen werden von einem bestimmten Client stammende HTTP-Anforderungen weiterhin von derselben Rolleninstanz behandelt.

Das Eclipse-Toolkit ermöglicht dies durch Installieren eines speziellen IIS-Moduls für den Netzwerklastenausgleich, Application Request Routing (ARR), in jeder der Rolleninstanzen. ARR leitet HTTP-Anforderungen an die entsprechende Rolleninstanz weiter. Das Toolkit konfiguriert automatisch den ausgewählten Endpunkt, damit der eingehende HTTP-Datenverkehr zunächst an die ARR-Software weitergeleitet wird. Das Toolkit erstellt außerdem einen neuen internen Endpunkt, der auf Ihrem Java-Server zum Abhören konfiguriert ist. Dies ist der Endpunkt, der von ARR für das Umleiten des HTTP-Datenverkehrs an die entsprechende Rolleninstanz verwendet wird. Auf diese Weise fungiert jede Rolleninstanz in der Mehrinstanzbereitstellung als Reverseproxy für alle anderen Instanzen, sodass persistente Sitzungen ermöglicht werden.

## <a name="notes-about-session-affinity"></a>Hinweise zur Sitzungsaffinität
* Sitzungsaffinität funktioniert nicht im Serveremulator. Die Einstellungen lassen sich im Serveremulator anwenden, ohne den Erstellungsprozess oder die Ausführung des Serveremulators zu stören, aber die Funktion selbst funktioniert im Serveremulator nicht.
* Das Aktivieren der Sitzungsaffinität führt zu einem erhöhten Speicherplatzbedarf der Bereitstellung in Azure, da zusätzliche Software heruntergeladen und in die Rolleninstanzen installiert wird, wenn der Dienst in der Azure-Cloud gestartet wird.
* Die Dauer der Initialisierung der einzelnen Rollen verlängert sich.
* Es wird ein interner Endpunkt zur Umleitung des Datenverkehrs hinzugefügt, wie oben beschrieben.

Ein Beispiel zum Verwalten von Sitzungsdaten bei aktivierter Sitzungsaffinität finden Sie im [Java Developer Center][How to Maintain Session Data with Session Affinity (in englischer Sprache)].

## <a name="see-also"></a>Weitere Informationen
[Azure-Toolkit für Eclipse][Azure-Toolkit für Eclipse]

[Create a Hello World Cloud Service for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse (in englischer Sprache)] (Erstellen eines Hello World-Clouddiensts für Azure in Eclipse)

[Installing the Azure Toolkit for Eclipse][Installation des Azure Toolkit für Eclipse] (Installieren des Azure-Toolkits für Eclipse) 

[Java Developer Center][How to Maintain Session Data with Session Affinity (in englischer Sprache)]

Weitere Informationen zum Verwenden von Azure mit Java finden Sie im [Azure Java Developer Center][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure-Toolkit für Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse (in englischer Sprache)]: http://go.microsoft.com/fwlink/?LinkID=699533
[How to Maintain Session Data with Session Affinity (in englischer Sprache)]: http://go.microsoft.com/fwlink/?LinkID=699539
[Installation des Azure Toolkit für Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->



<!--HONumber=Nov16_HO3-->


