---
title: "Hochladen eines benutzerdefinierten Images für Azure RemoteApp | Microsoft Docs"
description: "Erfahren Sie, wie Sie ein benutzerdefiniertes Image für Azure RemoteApp hochladen."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: 299e0510-1a6b-4fdf-914a-3631b061a360
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: ericor
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: b93861bc6277aaca5954bf9f4c338fed22a0c3fc
ms.lasthandoff: 04/27/2017


---
# <a name="upload-a-custom-image-for-azure-remoteapp"></a>Hochladen eines benutzerdefinierten Images für Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp wird am 31. August 2017 eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .
> 
> 

Nachdem Sie Ihr benutzerdefiniertes Vorlagenimage erstellt oder aktualisiert haben, müssen Sie es in Ihre Azure RemoteApp-Imagebibliothek hochladen. Gehen Sie dazu folgendermaßen vor:

## <a name="before-you-start"></a>Vorbereitung
1. Vergewissern Sie sich, dass Ihr benutzerdefiniertes Image den [Anforderungen an Images](remoteapp-imagereqs.md) und den [Anforderungen an Anwendungen](remoteapp-appreqs.md) entspricht.
2. Installieren Sie das [Azure PowerShell-Modul](/powershell/azure/overview).

## <a name="step-by-step-on-how-to-upload-custom-image"></a>So laden Sie ein benutzerdefiniertes Image hoch
1. Öffnen Sie das Azure-Verwaltungsportal, und navigieren Sie zur RemoteApp-Seite.
2. Klicken Sie unten auf der Registerkarte **Vorlagenimages** auf **Hochladen**.
3. Geben Sie einen aussagekräftigen Namen für Ihr Image und einen Speicherort im Speicherkonto an. Achten Sie darauf, dass der Speicherort mit dem Ihrer RemoteApp-Sammlung übereinstimmt (oder an dem Sie planen, sie zu erstellen).
4. Laden Sie, wenn Sie dazu aufgefordert werden, das Skript auf Ihren lokalen Computer herunter.
5. Kopieren Sie die Befehlsparameter im Textfeld in die Zwischenablage.
6. Öffnen Sie ein Windows PowerShell-Fenster mit erhöhten Rechten.
7. Navigieren Sie im Windows PowerShell-Fenster mit erhöhten Rechten zu dem Verzeichnis, in das Sie das Skript heruntergeladen haben.
8. Fügen Sie den kopierten Befehl ein, und drücken Sie die **Eingabetaste**.
   
   Der Upload wird gestartet. Die Dauer hängt von verschiedenen Faktoren wie der Netzwerkgeschwindigkeit und der Imagegröße ab.
9. Wenn der Upload aufgrund einer Netzwerkstörung oder einem ähnlichen Problem nicht abgeschlossen wird, können Sie den Vorgang jederzeit fortsetzen. Zum Fortsetzen eines Uploads führen Sie das Skript erneut mit derselben Befehlszeile aus.

> [!WARNING]
> Ändern Sie niemals das Uploadskript. Es sind verschiedene Tests implementiert, um sicherzustellen, dass das Image den Anforderungen an Images und Anwendungen entspricht.
> 
> 

## <a name="common-problems"></a>Häufiger auftretende Probleme
* Vergewissern Sie sich, dass Sie Windows PowerShell, nicht Azure PowerShell verwenden. Sie müssen das Azure PowerShell-Modul installieren, da bestimmte Module während des Uploads erforderlich sind.
* Nehmen Sie keine Änderungen am Skript vor, die Überprüfungen sollen Ihnen helfen.
* Wenn die VHD-Datei beim Upload ausgesperrt wird, kopieren oder verschieben Sie sie an den neuen Speicherort, und wiederholen Sie den Upload. Möglicherweise verhindern bestimmte Windows-Prozesse den Upload.  


