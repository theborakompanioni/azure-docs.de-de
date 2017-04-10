---
title: "Bereitstellen der gleichen Office 365-Umgebung auf jedem Gerät mit Azure RemoteApp | Microsoft Docs"
description: "Erfahren Sie, wie Sie mithilfe von Azure RemoteApp jede Office 365-App für Benutzer freigeben können."
services: remoteapp
documentationcenter: 
author: guscatalano
manager: mbaldwin
editor: 
ms.assetid: 0c971ce9-7d45-4cfb-9737-15b6706047e8
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 11/23/2016
ms.author: guscatal;elizapo
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 584c781c97097cda3c1455ade05cba8659f11073
ms.lasthandoff: 03/31/2017


---
# <a name="get-the-same-office-365-experience-on-any-device-with-azure-remoteapp"></a>Bereitstellen der gleichen Office 365-Umgebung auf jedem Gerät mit Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp wird am 31. August 2017 eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .
> 
> 

In diesem Artikel wird behandelt, wie Office 365 auf jedem Gerät in Ihrem Unternehmen bereitgestellt werden kann. Ihre Benutzer erhalten die gleichen Funktionen und die gleiche Benutzeroberfläche für Android, Apple und Windows.

Dies wird mithilfe von Azure RemoteApp erreicht, indem Office 365 auf skalierbaren virtuellen Computern in Azure gehostet wird, mit denen Benutzer eine Verbindung herstellen können. Diese Gruppe virtueller Computer nennen wir „Cloud-Sammlung“.

## <a name="create-a-cloud-collection"></a>Erstellen einer Cloud-Sammlung
Navigieren Sie nach der Erstellung eines Azure-Kontos zunächst zu **RemoteApp** , indem Sie im linken Bereich auf den entsprechenden Link klicken.
![Anzeigen von Azure RemoteApp im Azure-Portal](./media/remoteapp-tutorial-o365anywhere/1-menu.png)

Klicken Sie anschließend am unteren Rand auf **Neu** , und führen Sie eine Schnellerfassung für eine Sammlung durch. Geben Sie einen Namen, die Region, das Abonnement, den Plan und das von uns bereitgestellte Image „Office Professional 2013“ an.
![Dialogfeld "Erstellen"](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)

Sobald Sie alles eingetragen haben, sollte der Prozess der Sammlungserstellung beginnen. Dies kann bis zu einer Stunde dauern.

![Warten](./media/remoteapp-tutorial-o365anywhere/3-waiting.png)

Sobald der Vorgang abgeschlossen ist, sollte das Ergebnis wie folgt aussehen. Durch Klicken auf **Veröffentlichen** sehen Sie, dass die meisten Office-Anwendungen bereits veröffentlicht wurden.
![Sammlung erstellt](./media/remoteapp-tutorial-o365anywhere/4-done.png)

![Veröffentlichte Apps](./media/remoteapp-tutorial-o365anywhere/5-publish.png)

An diesem Punkt können Sie auch weitere Benutzer mit Zugriff auf diese Sammlung hinzufügen. Klicken Sie hierzu auf **Benutzerzugriff**.
![Konfigurieren des Benutzerzugriffs](./media/remoteapp-tutorial-o365anywhere/6-user.png)

Versuchen Sie jetzt, eine Verbindung mit Office 365 herzustellen.

## <a name="connect-to-office-365"></a>Herstellen einer Verbindung mit Office 365
Rufen Sie [https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/)auf, und klicken Sie auf **Client installieren**, um den Azure RemoteApp-Client auf dem verwendeten Gerät zu installieren. Die folgenden Screenshots beziehen sich auf Windows.

Beim Start der Anwendung werden Sie aufgefordert, sich mit Ihrem Microsoft-Konto (ehemals Live-ID) anzumelden. Verwenden Sie hierbei vorerst die gleichen Anmeldedaten wie für Ihr Azure-Konto. Nach erfolgreicher Anmeldung werden Sie auf neue Einladungen aufmerksam gemacht. Klicken Sie auf die Benachrichtigung, um eine Liste wie in der folgenden Abbildung anzuzeigen. Nehmen Sie die Einladung für die E-Mail-Adresse des Azure-Kontobesitzers an.

![Neue Einladung](./media/remoteapp-tutorial-o365anywhere/7-araclient.png)

So sieht es aus, wenn es neue Einladungen gibt.

![Akzeptieren einer Anwendung](./media/remoteapp-tutorial-o365anywhere/8-invitation.png)

Nachdem Sie die Einladung angenommen haben, werden alle Office-Apps im Azure RemoteApp-Client angezeigt.

![Liste der Apps](./media/remoteapp-tutorial-o365anywhere/9-work.png)

Klicken Sie auf die Anwendungen, um sie auf dem virtuellen Azure-Computer zu starten. Das war's! Viel Spaß!

![wird gestartet](./media/remoteapp-tutorial-o365anywhere/10-arastart.png)

![PowerPoint](./media/remoteapp-tutorial-o365anywhere/11-pp.png)


