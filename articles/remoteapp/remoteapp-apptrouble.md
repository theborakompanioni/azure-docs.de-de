---
title: "Problembehandlung bei Azure RemoteApp – Fehler beim Starten und Verbinden der Anwendung | Microsoft Docs"
description: Erfahren Sie, wie Sie Problemen beim Starten von Anwendungen und Herstellen einer Verbindung mit Anwendungen in Azure RemoteApp beheben.
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: e5cf7171-d1c2-4053-a38b-5af7821305e1
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 6bc1325809f39a45d387914e1fdc4d10f46df539
ms.lasthandoff: 03/31/2017


---
# <a name="troubleshoot-azure-remoteapp---application-launch-and-connection-failures"></a>Problembehandlung bei Azure RemoteApp – Fehler beim Starten und Verbinden der Anwendung
> [!IMPORTANT]
> Azure RemoteApp wird am 31. August 2017 eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .
> 
> 

Anwendungen, die in Azure RemoteApp gehostet werden, können aus unterschiedlichen Gründe nicht gestartet werden. In diesem Artikel werden die verschiedenen Gründe und Fehlermeldungen beschrieben, die Benutzern möglicherweise ausgegeben werden, wenn sie versuchen, Anwendungen zu starten. Er befasst sich auch mit Verbindungsfehlern. (In diesem Artikel werden jedoch keine Probleme mit der Anmeldung beim Azure RemoteApp-Client beschrieben.)  

Im Folgenden finden Sie Informationen zu Fehlermeldungen zu Fehlern beim Starten oder Verbinden von Apps.

## <a name="were-getting-you-set-up-try-again-in-10-minutes"></a>Die Einrichtung erfolgt jetzt … Versuchen Sie es in 10 Minuten noch einmal.
Dieser Fehler weist darauf hin, dass Azure RemoteApp eine zentrale Hochskalierung durchführt, um die erforderliche Kapazität für die Benutzer bereitzustellen. Im Hintergrund werden weitere Azure RemoteApp-VM-Instanzen erstellt, um die Kapazitätsanforderungen Ihrer Benutzer zu erfüllen. Dies dauert in der Regel ca. fünf Minuten, es kann aber auch bis zu 10 Minuten in Anspruch nehmen. In manchen Fällen erfolgt diese Skalierung nicht schnell genug, und die Ressourcen werden sofort benötigt. Ein Beispiel hierfür wäre ein 9-Uhr-Szenario, bei dem viele Benutzer Ihre App gleichzeitig in Azure RemoteApp verwenden müssen. Wenn dies auf Sie zutrifft, können wir auf dem Back-End den **Kapazitätsmodus** aktivieren. Dazu öffnen Sie ein Azure-Support-Ticket. Geben Sie unbedingt Ihre Abonnement-ID in der Anforderung an.  

![Die Einrichtung erfolgt jetzt …](./media/remoteapp-apptrouble/ra-apptrouble1.png)

## <a name="could-not-auto-reconnect-to-your-applications-please-re-launch-your-application"></a>Es konnte nicht automatisch eine erneute Verbindung mit Ihrer Anwendung hergestellt werden. Starten Sie Ihre Anwendung neu.
Diese Fehlermeldung wird häufig angezeigt, wenn Sie Azure RemoteApp verwenden, dann Ihren PC länger als vier Stunden in den Standbymodus versetzen und anschließend wieder aktivieren. Der Azure RemoteApp-Client versucht dann automatisch, erneut eine Verbindung herzustellen, wobei das Timeout überschritten wird.  Weisen Sie die Benutzer an, zurück zur Anwendung zu wechseln und zu versuchen, sie aus dem Azure RemoteApp-Client zu öffnen.

![Es konnte nicht automatisch eine erneute Verbindung mit Ihrer Anwendung hergestellt werden](./media/remoteapp-apptrouble/ra-apptrouble2.png) 

## <a name="problems-with-the-temp-profile"></a>Probleme mit dem temporären Profil
Dieser Fehler tritt auf, wenn Ihr Benutzerprofil (Benutzerprofil-Datenträger) nicht bereitgestellt wurde, und der Benutzer ein temporäres Profil erhält.  Administratoren sollten zur Auflistung im Azure-Portal navigieren und dann auf der Registerkarte **Sitzungen** versuchen, den **Benutzer abzumelden**. Damit wird die vollständige Abmeldung der Benutzersitzung erzwungen, sodass der Benutzer die App danach neu starten muss. Wenn dies nicht möglich ist, wenden Sie sich an den Azure-Support.

## <a name="azure-remoteapp-has-stopped-working"></a>Azure RemoteApp funktioniert nicht mehr
Diese Fehlermeldung bedeutet, dass beim Azure RemoteApp-Client ein Problem aufgetreten ist und er neu gestartet werden muss. Weisen Sie die Benutzer an, den Client zu schließen: Wählen Sie **Programm schließen** aus, und starten Sie den Azure RemoteApp-Client erneut.  Wenn das Problem weiterhin auftritt, öffnen Sie ein Azure-Support-Ticket.

![Azure RemoteApp funktioniert nicht mehr](./media/remoteapp-apptrouble/ra-apptrouble3.png)  

## <a name="an-error-occurred-while-remote-desktop-connection-was-accessing-this-resource-retry-the-connection-or-contact-your-system-administrator"></a>Fehler beim Zugriff der Remotedesktopverbindung auf die Ressource. Versuchen Sie noch einmal, eine Verbindung herzustellen, oder wenden Sie sich an den Netzwerkadministrator.
Dies ist eine allgemeine Fehlermeldung. Wenden Sie sich an den Azure-Support, damit wir das Problem untersuchen können. 

![Generische Azure RemoteApp-Meldung](./media/remoteapp-apptrouble/ra-apptrouble4.png) 


