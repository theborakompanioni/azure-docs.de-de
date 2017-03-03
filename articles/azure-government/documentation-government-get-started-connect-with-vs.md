---
title: "Herstellen einer Verbindung mit Azure Government über Visual Studio | Microsoft Docs"
description: "Informationen zum Verwalten Ihres Abonnements in Azure Government durch Herstellen einer Verbindung über Visual Studio"
services: azure-government
cloud: gov
documentationcenter: 
author: zakramer
manager: liki
ms.assetid: faf269aa-e879-4b0e-a5ba-d4110684616a
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 02/13/2017
ms.author: zakramer
translationtype: Human Translation
ms.sourcegitcommit: 6b29e98aa3458a76e186234c2e9800294902bea9
ms.openlocfilehash: d49b2c6afc48cda01045cddb3844c5234e72de37
ms.lasthandoff: 02/21/2017


---


# <a name="connecting-via-visual-studio"></a>Herstellen der Verbindung über Visual Studio
Visual Studio wird von Entwicklern beim Erstellen von Lösungen zur einfachen Verwaltung ihrer Azure-Abonnements verwendet.  Visual Studio lässt die Konfiguration einer Verbindung mit Azure Government auf der Benutzeroberfläche derzeit nicht zu.  

### <a name="updating-visual-studio-for-azure-government"></a>Aktualisieren von Visual Studio für Azure Government
Sie müssen die Registrierung aktualisieren, damit Visual Studio eine Verbindung mit Azure Government zu herstellen kann.

1. Schließen Sie Visual Studio.
2. Erstellen Sie eine Textdatei mit dem Namen **VisualStudioForAzureGov.reg**.
3. Kopieren Sie den folgenden Text in **VisualStudioForAzureGov.reg**:
   
        Windows Registry Editor Version 5.00
   
        [HKEY_CURRENT_USER\Software\Microsoft\VSCommon\ConnectedUser]
        "AadInstance"="https://login-us.microsoftonline.com/"
        "adaluri"="https://management.core.usgovcloudapi.net"
        "AzureRMEndpoint"="https://management.usgovcloudapi.net"
        "AzureRMAudienceEndpoint"="https://management.core.usgovcloudapi.net"
        "EnableAzureRMIdentity"="true"
        "GraphUrl"="graph.windows.net"
4. Speichern Sie die Datei und führen sie anschließend durch Doppelklick aus.  Sie werden aufgefordert, die Datei in der Registrierung zusammenzuführen.
5. Starten Sie Visual Studio, und beginnen Sie mit der Verwendung von [Cloud-Explorer](../vs-azure-tools-resources-managing-with-cloud-explorer.md).

> [!NOTE]
> Nach dem Festlegen dieses Registrierungsschlüssels sind nur Azure Government-Abonnements zugänglich.  Zuvor konfigurierte Abonnements werden zwar noch angezeigt, funktionieren jedoch nicht, da Visual Studio jetzt mit Azure Government anstatt Azure Public verbunden ist.  Im folgenden Abschnitt finden Sie die Schritte zum Rückgängigmachen der Änderungen.
> 
> 

### <a name="reverting-visual-studio-connection-to-azure-government"></a>Zurücksetzen der Visual Studio-Verbindung mit Azure Government
Damit Visual Studio eine Verbindung mit Azure Public herstellen kann, müssen Sie die Registrierungseinträge entfernen, die eine Verbindung mit Azure Government ermöglichen.

1. Schließen Sie Visual Studio.
2. Erstellen Sie eine Textdatei mit dem Namen **VisualStudioForAzureGov_Remove.reg**.
3. Kopieren Sie den folgenden Text in **VisualStudioForAzureGov_Remove.reg**:
   
        Windows Registry Editor Version 5.00
   
        [HKEY_CURRENT_USER\Software\Microsoft\VSCommon\ConnectedUser]
        "AadInstance"=-
        "adaluri"=-
        "AzureRMEndpoint"=-
        "AzureRMAudienceEndpoint"=-
        "EnableAzureRMIdentity"=-
        "GraphUrl"=-
4. Speichern Sie die Datei und führen sie anschließend durch Doppelklick aus.  Sie werden aufgefordert, die Datei in der Registrierung zusammenzuführen.
5. Starten Sie Visual Studio.

> [!NOTE]
> Nach dem Zurücksetzen dieses Registrierungsschlüssels werden Ihre Azure Government-Abonnements zwar angezeigt, der Zugriff darauf ist jedoch nicht möglich.  Sie können problemlos entfernt werden.
> 
> 

