---
title: "Behandlung häufig auftretender Probleme bei der Erstellung virtueller Festplatten | Microsoft-Dokumentation"
description: "Hier finden Sie Antworten auf häufig gestellte Fragen zur Problembehandlung und zu Problemen bei der Erstellung virtueller Festplatten."
services: Azure Marketplace
documentationcenter: 
author: HannibalSII
manager: 
editor: 
ms.assetid: e39563d8-8646-4cb7-b078-8b10ac35b494
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 09/26/2016
ms.author: hascipio; v-divte
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: c4e88a9fbb15dd90d619b159ae1065dfacc1907f
ms.lasthandoff: 03/31/2017


---
# <a name="how-to-troubleshoot-common-issues-encountered-during-vhd-creation"></a>Behandlung häufig auftretender Probleme bei der Erstellung virtueller Festplatten
Dieser Artikel soll Azure Marketplace-Herausgeber und/oder Co-Administratoren unterstützen, die Probleme oder allgemeine Fragen beim Veröffentlichen oder Verwalten ihrer Lösungen mit virtuellen Computern haben.

1. Wie ändere ich den Namen des Hosts?
   
    Nach der Erstellung des virtuellen Computers können Benutzer den Namen des Hosts nicht aktualisieren.
2. Wie wird der Remotedesktopdienst oder sein Anmeldekennwort zurückgesetzt?
   
   * [Referenz für virtuelle Windows-Computer](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-reset-rdp/)
   * [Referenz für virtuelle Linux-Computer](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)
3. Wie werden neue SSH-Zertifikate generiert?
   
   Informationen finden Sie unter folgendem Link: [https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)
4. Wie wird ein offenes VPN-Zertifikat konfiguriert?
   
   Informationen finden Sie unter folgendem Link: [https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/](https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/)
5. Welche Unterstützungsrichtlinie gilt für die Ausführung von Microsoft-Serversoftware in der Umgebung für virtuelle Microsoft Azure-Computer (Infrastructure-as-a-Service)?
   
   Informationen finden Sie unter folgendem Link: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)
6. Besitzen virtuelle Computer einen eindeutigen Bezeichner?
   
   Von Azure wird auf jedem virtuellen Computer eine eindeutige Azure-VM-ID codiert. Details hierzu finden Sie in diesem Blog und dieser Dokumentation.
7. Wie wird auf einem virtuellen Computer die benutzerdefinierte Skripterweiterung in der Startaufgabe verwaltet?
   
   Informationen finden Sie unter folgendem Link: [https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/)
8. Wie erstelle ich mithilfe der virtuellen Festplatte, die in Storage Premium hochgeladen wird, einen virtuellen Computer im Azure-Portal?
   
   Dieses Feature wird noch nicht unterstützt.
9. Wird im Azure Marketplace eine 32-Bit-App unterstützt?
   
   Ausführliche Informationen zur Unterstützungsrichtlinie finden Sie unter folgendem Link: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)
10. Bei jeder Erstellung eines Images aus virtuellen Festplatten erhalte ich in PowerShell eine Fehlermeldung mit dem Hinweis, dass die virtuelle Festplatte bereits mit dem Image-Repository als Ressource registriert ist. Ich habe vorher weder ein Image erstellt noch habe ich ein Image mit diesem Namen in Azure gefunden. Wie löse ich dieses Problem?
    
    Dieses Problem tritt in der Regel auf, wenn der Benutzer einen virtuellen Computer von dieser virtuellen Festplatte bereitgestellt hat und für diese eine Sperre festgelegt wurde. Vergewissern Sie sich, dass von dieser virtuellen Festplatte kein virtueller Computer zugewiesen wurde. Falls der Fehler weiterhin auftritt, erstellen Sie über den folgenden Link oder im Veröffentlichungsportal ein Supportticket (Details finden Sie in der Antwort zu Frage 11).
