---
title: Problembehebung von Schutzfehlern (VMware/physisch auf Azure) | Microsoft-Dokumente
description: Dieser Artikel beschreibt allgemeine Replikationsfehler von VMware-Computern und die entsprechenden Schritte zur Problembehebung.
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/26/2017
ms.author: asgang
ms.translationtype: Human Translation
ms.sourcegitcommit: 07584294e4ae592a026c0d5890686eaf0b99431f
ms.openlocfilehash: 6ebec2e06566b1e2d6834fdd81c0d8b2801b80b9
ms.contentlocale: de-de
ms.lasthandoff: 06/01/2017


---
# <a name="troubleshoot-on-premises-vmwarephysical-server-replication-issues"></a>Problembehebung von Replikationsfehlern bei lokalen VMware-Servern/physischen Servern
Sie erhalten unter Umständen eine bestimmte Fehlermeldung, wenn Sie ihre virtuellen VMware-Computer oder Ihre physischen Server mithilfe von Azure Site Recovery schützen. In diesem Artikel werden einige der häufigeren Fehlermeldungen beschrieben, zusammen mit Problemlösungsschritten zur Fehlerbehebung.


## <a name="initial-replication-is-stuck-at-0"></a>Die erste Replikation geht nicht über 0% hinaus.
Die meisten Fehler bei der ersten Replikation treten aufgrund von Konnektivitätsproblemen zwischen dem Quellserver und dem Prozessserver oder zwischen dem Prozessserver und Azure auf.
In den meisten Fällen können Sie dieses Problem selbst beheben, indem Sie die folgenden Schritte ausführen.

###<a name="check-the-following-on-source-machine"></a>Überprüfen Sie Folgendes auf dem QUELLCOMPUTER:
* Verwenden Sie in der Befehlszeile des Quellservercomputers wie unten gezeigt Telnet zum Pingen des Prozessservers mit dem https-Port (Standard: 9443), um festzustellen, ob Netzwerkkonnektivitätsprobleme bestehen oder der Port durch die Firewall blockiert wird.
     
    `telnet <PS IP address> <port>`
> [!NOTE]
    > Testen Sie die Konnektivität mit Telnet und nicht mit PING.  Wenn Telnet nicht installiert ist, führen Sie die [hier](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx) angegebenen Schritte aus.

Wenn keine Verbindung hergestellt werden kann, lassen Sie den eingehenden Port 9443 auf dem Prozessserver zu, und überprüfen Sie, ob das Problem weiterhin besteht. In der Vergangenheit wurde dieses Problem bereits dadurch hervorgerufen, dass sich der Prozessserver hinter DMZ befunden hat.

* Prüfen Sie den Status des Diensts `InMage Scout VX Agent – Sentinel/OutpostStart`, wenn er nicht ausgeführt wird, und überprüfen Sie, ob das Problem weiterhin besteht.   
 
###<a name="check-the-following-on-process-server"></a>Überprüfen Sie Folgendes auf dem PROZESSSERVER:

* **Überprüfen Sie, ob der Prozessserver aktiv Daten mithilfe von Push an Azure überträgt.** 

Öffnen Sie den Task-Manager auf dem Prozessservercomputer (betätigen Sie die Tasten STRG + UMSCHALT + ESC). Wechseln Sie zur Registerkarte „Leistung“, und klicken Sie auf den Link „Ressourcenmonitor öffnen“. Wechseln Sie ausgehend vom Resource Manager zur Registerkarte „Netzwerk“. Prüfen Sie ob „cbengine.exe“ unter „Prozesse mit Netzwerkaktivität“ aktiv große Datenmengen (in MB) sendet.

![Replikation aktivieren](./media/site-recovery-protection-common-errors/cbengine.png)

Führen Sie andernfalls die folgenden Schritte aus:

* **Prüfen Sie, ob der Prozessserver eine Verbindung mit Azure Blob herstellen kann**: Wählen Sie „cbengine.exe“ aus, und überprüfen Sie unter „TCP-Verbindungen“, ob zwischen dem Prozessserver und der Azure Storage Blob-URL eine Verbindung besteht.

![Replikation aktivieren](./media/site-recovery-protection-common-errors/rmonitor.png)

Navigieren Sie andernfalls zur Systemsteuerung > „Dienste“, und prüfen Sie, ob die folgenden Dienste ausgeführt werden:

     * cxprocessserver
     * InMage Scout VX Agent – Sentinel/Outpost
     * Microsoft Azure Recovery Services Agent
     * Microsoft Azure Site Recovery Service
     * tmansvc
     * 
Starten Sie alle nicht ausgeführten Dienste (bzw. starten Sie sie neu), und überprüfen Sie, ob das Problem weiterhin besteht.

* **Prüfen Sie, ob der Prozessserver mithilfe von Port 443 eine Verbindung mit der öffentlichen Azure-IP-Adresse herstellen kann.**

Öffnen Sie das aktuelle Fehlerprotokoll (CBEngineCurr.errlog) unter `%programfiles%\Microsoft Azure Recovery Services Agent\Temp`, und suchen Sie nach „:443“ und „connection attempt failed“.

![Replikation aktivieren](./media/site-recovery-protection-common-errors/logdetails1.png)

Wenn Probleme auftreten, verwenden Sie in der Befehlszeile des Prozessservers Telnet zum Pingen Ihrer öffentlichen Azure-IP-Adresse (im der Abbildung oben unleserlich gemacht), die Sie mithilfe von Port 443 in „CBEngineCurr.currLog“ gefunden haben.

      telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443
Wenn Sie keine Verbindung herstellen können, prüfen Sie, ob das Zugriffsproblem durch die Firewall oder den Proxy verursacht wird.


* **Prüfen Sie, ob die auf einer IP-Adresse basierende Firewall auf dem Prozessserver den Zugriff blockiert**: Wenn Sie auf IP-Adressen basierende Firewallregeln auf dem Server verwenden, laden Sie [hier](https://www.microsoft.com/download/details.aspx?id=41653) die vollständige Liste der IP-Bereiche für das Microsoft Azure-Rechenzentrum herunter, und fügen Sie diese Ihrer Firewallkonfiguration hinzu, um sicherzustellen, dass eine Kommunikation mit Azure (und dem HTTPS (443)-Port) zulässig ist.  Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ (wird für Zugriffsteuerung und Identitätsverwaltung verwendet) zu.

* **Prüfen Sie, ob eine URL-basierte Firewall auf dem Prozessserver den Zugriff blockiert**: Wenn Sie URL-basierte Firewallregeln auf dem Server verwenden, stellen Sie sicher, dass die folgenden URLs zur Firewallkonfiguration hinzugefügt wurden. 
     
  `*.accesscontrol.windows.net:`: Wird für Access Control und Identitätsverwaltung verwendet.

  `*.backup.windowsazure.com:`: Wird für die Übertragung und Orchestrierung von Replikationsdaten verwendet.

  `*.blob.core.windows.net:`: Wird für den Zugriff auf das Speicherkonto zum Speichern von replizierten Daten verwendet.

  `*.hypervrecoverymanager.windowsazure.com:`: Wird für die Vorgänge und Orchestrierung der Replikationsverwaltung verwendet.

  `time.nist.gov` und `time.windows.com`: Wird zum Überprüfen der Zeitsynchronisierung zwischen Systemzeit und der globalen Zeit verwendet.

URLs für **Azure Government Cloud**:

`* .ugv.hypervrecoverymanager.windowsazure.us`

`* .ugv.backup.windowsazure.us`

`* .ugi.hypervrecoverymanager.windowsazure.us`

`* .ugi.backup.windowsazure.us` 

* **Prüfen Sie, ob die Proxyeinstellungen auf dem Prozessserver den Zugriff blockieren**.  Wenn Sie einen Proxyserver verwenden, stellen Sie sicher, dass der Proxyservername durch den DNS-Server aufgelöst wird.
So überprüfen Sie, was Sie bei der Einrichtung des Konfigurationsservers angegeben haben: Navigieren Sie zum Registrierungsschlüssel:

    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings`

Stellen Sie nun sicher, dass die gleichen Einstellungen vom Azure Site Recovery-Agent zum Senden von Daten verwendet werden.
Suchen Sie nach „Microsoft Azure Backup“. 

![Replikation aktivieren](./media/site-recovery-protection-common-errors/mab.png)

Klicken Sie nach dem Öffnen auf „Aktion“ > „Eigenschaften ändern“. Auf der Registerkarte „Proxykonfiguration“ wird Ihnen die Proxyadresse angezeigt, die mit der unter den Registrierungseinstellungen angezeigten Adresse übereinstimmen muss. Wenn dies nicht der Fall ist, passen Sie die Adresse an.

![Replikation aktivieren](./media/site-recovery-protection-common-errors/mabproxy.png)

* **Prüfen Sie, ob die Bandbreite auf dem Prozessserver eingeschränkt ist**: Erhöhen Sie die Bandbreite, und überprüfen Sie, ob das Problem weiterhin besteht.

##<a name="next-steps"></a>Nächste Schritte
Wenn Sie weitere Unterstützung benötigen, können Sie Ihre Frage im [ASR-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr) veröffentlichen. Unsere Community ist sehr aktiv, und einer unserer Techniker wird Sie bei Ihrem Problem unterstützen.
