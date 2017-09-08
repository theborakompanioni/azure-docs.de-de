---
title: Beheben von Problemen bei einem Failover nach Azure | Microsoft-Dokumentation
description: "Dieser Artikel beschreibt Möglichkeiten zum Beheben allgemeiner Probleme beim Failover nach Azure."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/24/2017
ms.author: pratshar
ms.translationtype: HT
ms.sourcegitcommit: 8351217a29af20a10c64feba8ccd015702ff1b4e
ms.openlocfilehash: 0e50433e1ccdcbc0010070eec110914f0d33b5ab
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---
# <a name="troubleshoot-errors-when-failing-over-a-virtual-machine-to-azure"></a>Beheben von Problemen beim Ausführen eines Failovers eines virtuellen Computers nach Azure
Bei einem Failover eines virtuellen Computers nach Azure tritt eventuell einer der folgenden Fehler auf. Gehen Sie für die Problembehandlung gemäß den für den jeweiligen Fehlerzustand beschriebenen Schritten vor.


## <a name="failover-failed-with-error-id-28031"></a>Fehler beim Failover mit der Fehler-ID 28031

Site Recovery konnte einen virtuellen Computer, für den ein Failover ausgeführt wurde, nicht in Azure erstellen. Dies kann aus einem der folgenden Gründe auftreten:

* Das verfügbare Kontingent reicht nicht zum Erstellen des virtuellen Computers: Sie können das verfügbare Kontingent unter „Abonnement“ > „Nutzung + Kontingente“ prüfen. Sie können eine [neue Supportanfrage](http://aka.ms/getazuresupport) eröffnen, um das Kontingent zu erhöhen.
     
* Sie versuchen, ein Failover für virtuelle Computer unterschiedlicher Größensätze in derselben Verfügbarkeitsgruppe durchzuführen. Verwenden Sie unbedingt dieselben Größen Familien für alle virtuellen Computer in derselben Verfügbarkeitsgruppe. Sie können die Größe ändern, indem Sie zu den Einstellungen für Compute und Netzwerk des virtuellen Computers navigieren. Wiederholen Sie das Failover anschließend.
  
* Eine Richtlinie für das Abonnement verhindert die Erstellung eines virtuellen Computers. Ändern Sie die Richtlinie, um das Erstellen eines virtuellen Computers zuzulassen, und wiederholen dann das Failover. 

## <a name="failover-failed-with-error-id-28092"></a>Fehler beim Failover mit der Fehler-ID 28092

Site Recovery konnte für den virtuellen Computer, für den ein Failover ausgeführt wurde, keine Netzwerkschnittstelle erstellen. Stellen Sie sicher, dass Sie in Ihrem Abonnement über ein ausreichendes Kontingent zum Erstellen von Netzwerkschnittstellen verfügen. Sie können das verfügbare Kontingent unter „Abonnement“ > „Nutzung + Kontingente“ prüfen. Sie können eine [neue Supportanfrage](http://aka.ms/getazuresupport) eröffnen, um das Kontingent zu erhöhen. Wenn Sie über ein ausreichendes Kontingent verfügen, ist dies möglicherweise nur ein vorübergehendes Problem. Wiederholen Sie den Vorgang. Wenn das Problem auch nach Wiederholungen weiter besteht, hinterlassen Sie am Ende dieses Dokuments einen Kommentar.  

## <a name="failover-failed-with-error-id-70038"></a>Fehler beim Failover mit der Fehler-ID 70038

Site Recovery konnte einen klassischen virtuellen Computer, für den ein Failover ausgeführt wurde, nicht in Azure erstellen. Dies kann mehrere Ursachen haben:

* Eine der Ressourcen – z.B. ein virtuelles Netzwerk, das für die Erstellung des virtuellen Computers erforderlich ist – ist nicht vorhanden. Erstellen Sie das virtuelle Netzwerk wie in den Einstellungen für Compute und Netzwerk des virtuellen Computers angegeben, oder ändern Sie die Einstellung in ein virtuelles Netzwerk, das bereits vorhanden ist. Wiederholen Sie dann das Failover. 


## <a name="next-steps"></a>Nächste Schritte

Wenn Sie weitere Hilfe benötigen, veröffentlichen Sie Ihre Abfrage im [Site Recovery-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr), oder hinterlassen einen Kommentar am Ende dieses Dokuments. Wir verfügen über eine aktive Community, die Sie unterstützen kann.
