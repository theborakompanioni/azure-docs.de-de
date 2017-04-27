---
title: "Veröffentlichen einer App in Azure RemoteApp | Microsoft Docs"
description: "Erfahren Sie, wie Sie Anwendungen und Ressourcen in Azure RemoteApp veröffentlichen."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: c7e1a2cd-8e1f-4a33-bd43-8032ec9ac952
ms.service: remoteapp
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 4cd4d35f44320ac57f015b5444985e8b4976ccf0
ms.lasthandoff: 03/31/2017


---
# <a name="how-to-publish-an-app-in-remoteapp"></a>Gewusst wie: Veröffentlichen einer App in RemoteApp
> [!IMPORTANT]
> Azure RemoteApp wird am 31. August 2017 eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .
> 
> 

Nach dem Erstellen der RemoteApp-Sammlung müssen Sie die Apps oder Ressourcen, die Sie für Ihre Benutzer zur Verfügung stellen möchten, veröffentlichen. Die Vorlagenimages, die mit Ihrem Abonnement bereitgestellt wurden, enthalten nur wenige Apps, die standardmäßig veröffentlicht werden. Um die anderen Apps freizugeben, müssen Sie diese zuerst veröffentlichen.

> [!NOTE]
> Müssen Sie eine App aktualisieren? Sie müssen zunächst [das Image aktualisieren](remoteapp-update.md) .
> 
> 

Klicken Sie im Portal auf der **Registerkarte Veröffentlichung** auf **Veröffentlichen**. Sie können entweder eine App aus dem **Startmenü** des Vorlagenimages hinzufügen oder den Pfad angeben, in dem die App auf dem Vorlagenimage installiert ist. Wenn Sie die App aus dem **Startmenü** hinzufügen möchten, wählen Sie die zu veröffentlichende App in der Liste aus. Wenn Sie den Pfad zur App bereitstellen möchten, geben Sie einen Namen für die App und den Pfad zur App an. Verwenden Sie Variablen im Pfad, z.B. "%systemdrive%" anstelle von "c:\".

> [!NOTE]
> Wenn Sie die App aus dem **Startmenü** hinzufügen möchten, müssen Sie *diese App im Vorlagenimage dem **Startmenü** hinzugefügt haben.* Andernfalls wird in RemoteApp nur angezeigt, was sich *im* **Startmenü** befindet, was zu Verwechslungen führen kann. 
> 
> Um sicherzustellen, dass sich die App im **Startmenü** befindet, platzieren Sie eine Verknüpfungsdatei (**.lnk**) im Ordner „%systemdrive%\ProgramData\Microsoft\Windows\Start“.
> 
> Wenn Sie beim Erstellen der Vorlage vergessen haben, die App dem **Startmenü** hinzufügen, fügen Sie der App den Pfad hinzu. (Oder erstellen Sie Ihr Vorlagenimage neu. Dies bedeutet allerdings einen etwas höheren Arbeitsaufwand.)
> 
> 


