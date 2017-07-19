---
title: "Azure Site Recovery-Problembehandlung für Azure-zu-Azure-Replikationsprobleme und -fehler| Microsoft-Dokumentation"
description: "Problembehandlungsfehler und -probleme beim Replizieren von virtuellen Azure-Computern für die Notfallwiederherstellung"
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/10/2017
ms.author: sujayt
ms.translationtype: Human Translation
ms.sourcegitcommit: a1ba750d2be1969bfcd4085a24b0469f72a357ad
ms.openlocfilehash: 4e4e8b097fbab3ddce551eb93945d0880f8c457f
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017


---
# <a name="troubleshoot-azure-to-azure-vm-replication-issues"></a>Problembehandlung für Azure-zu-Azure-VM-Replikationsprobleme

In diesem Artikel werden die Probleme beschrieben, die in Azure Site Recovery beim Replizieren und Wiederherstellen von virtuellen Azure-Computern aus einer Region in einer anderen Region häufiger auftreten können, und es wird erklärt, wie die Problembehandlung durchgeführt werden kann. Weitere Informationen zu unterstützten Konfigurationen finden Sie unter [Azure Site Recovery support matrix for replicating from Azure to Azure](site-recovery-support-matrix-azure-to-azure.md) (Azure Site Recovery-Supportmatrix zum Replizieren aus Azure in Azure).

## <a name="azure-resource-quota-issues-error-code-150097"></a>Probleme mit dem Azure-Ressourcenkontingent (Fehlercode 150097)
Für Ihr Abonnement sollte es möglich sein, Azure-VMs in der Zielregion zu erstellen, die Sie als Region für die Notfallwiederherstellung nutzen möchten. Außerdem sollte für Ihr Abonnement ein ausreichendes Kontingent für die Erstellung von VMs einer bestimmten Größe vorhanden sein. Site Recovery wählt für die Ziel-VM und die Quell-VM standardmäßig die gleiche Größe. Falls keine übereinstimmende Größe verfügbar ist, wird automatisch die nächstgelegene Größe gewählt. Wenn keine übereinstimmende Größe vorhanden ist, für die die Konfiguration der Quell-VM unterstützt wird, wird diese Fehlermeldung angezeigt:

**Fehlercode** | **Mögliche Ursachen** | **Empfehlung**
--- | --- | ---
150097<br></br>**Meldung**: Die Replikation konnte für den virtuellen Computer VmName nicht aktiviert werden. | - Für Ihre Abonnement-ID ist die Erstellung von VMs am Standort der Zielregion ggf. nicht aktiviert.</br></br>- Für Ihre Abonnement-ID ist die Erstellung von bestimmten VM-Größen am Standort der Zielregion ggf. nicht aktiviert, oder es ist kein ausreichendes Kontingent vorhanden.</br></br>- Für die Abonnement-ID ist am Standort der Zielregion keine geeignete Größe der Ziel-VM vorhanden, die mit der NIC-Anzahl (2) der Quell-VM übereinstimmt.| Wenden Sie sich unter [Anforderungen zur Erhöhung von Resource Manager-Kernkontingenten](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) an den Azure-Abrechnungssupport, um die VM-Erstellung für die erforderlichen VM-Größen am Zielstandort für Ihr Abonnement aktivieren zu lassen. Wiederholen Sie den Vorgang, nachdem die Aktivierung durchgeführt wurde.

### <a name="fix-the-problem"></a>Beheben des Problems
Sie können sich unter [Anforderungen zur Erhöhung von Resource Manager-Kernkontingenten](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) an den Azure-Abrechnungssupport wenden, um Ihr Abonnement für die Erstellung von VMs der erforderlichen Größen am Zielstandort aktivieren zu lassen.

Falls für den Zielstandort eine Kapazitätsbeschränkung besteht, sollten Sie die Replikation deaktivieren und für einen anderen Standort aktivieren, an dem für Ihr Abonnement ein ausreichendes Kontingent zum Erstellen von VMs der erforderlichen Größen zur Verfügung steht.

## <a name="trusted-root-certificates-error-code-151066"></a>Vertrauenswürdige Stammzertifikate (Fehlercode: 151066)

Wenn auf der VM nicht alle aktuellen vertrauenswürdigen Stammzertifikate vorhanden sind, ist der Auftrag „Replikation aktivieren“ ggf. nicht erfolgreich. Ohne die Zertifikate tritt für die Authentifizierung und Autorisierung von Site Recovery-Dienstaufrufen über die VM ein Fehler auf. Die Fehlermeldung für den fehlgeschlagenen Site Recovery-Auftrag „Replikation aktivieren“ wird angezeigt:

**Fehlercode** | **Mögliche Ursache** | **Empfehlungen**
--- | --- | ---
151066<br></br>**Meldung**: Fehler bei der Site Recovery-Konfiguration. | Die erforderlichen vertrauenswürdigen Stammzertifikate, die für die Autorisierung und Authentifizierung verwendet werden, sind auf dem Computer nicht vorhanden. | - Für eine VM, auf der das Windows-Betriebssystem ausgeführt wird, sollten Sie sicherstellen, dass die vertrauenswürdigen Stammzertifikate auf dem Computer vorhanden sind. Informationen finden Sie unter [Konfigurieren vertrauenswürdiger Stämme und unzulässiger Zertifikate](https://technet.microsoft.com/library/dn265983.aspx).<br></br>- Befolgen Sie für eine VM, auf der das Linux-Betriebssystem ausgeführt wird, die Anleitung für vertrauenswürdige Stammzertifikate. Diese Anleitung wird vom Distributor der Linux-Betriebssystemversion veröffentlicht.

### <a name="fix-the-problem"></a>Beheben des Problems
**Windows**

Installieren Sie alle aktuellen Windows-Updates auf der VM, damit alle vertrauenswürdigen Stammzertifikate auf dem Computer vorhanden sind. Wenn Sie sich in einer nicht verbundenen Umgebung befinden, können Sie den Standardprozess für Windows-Updates in Ihrer Organisation durchführen, um die Zertifikate abzurufen. Falls die erforderlichen Zertifikate auf der VM nicht vorhanden sind, schlagen die Aufrufe des Site Recovery-Diensts aus Sicherheitsgründen fehl.

Führen Sie den üblichen Prozess Ihrer Organisation für die Verwaltung von Windows-Updates oder Zertifikaten durch, um alle aktuellen Stammzertifikate und die aktualisierte Zertifikatssperrliste auf den VMs abzurufen.

Navigieren Sie mit einem Browser auf Ihrer VM zu „login.microsoftonline.com“, um zu überprüfen, ob das Problem behoben wurde.

**Linux**

Befolgen Sie die Anleitung Ihres Linux-Distributors, um die aktuellen vertrauenswürdigen Stammzertifikate und die aktuelle Zertifikatssperrliste auf der VM abzurufen.

Führen Sie diese Schritte aus, da SuSE Linux zum Verwalten einer Zertifikatliste symlinks verwendet:

1.  Melden Sie sich als Stammbenutzer an.

2.  Führen Sie den folgenden Befehl aus:

      ``# cd /etc/ssl/certs``

3.  Führen Sie diesen Befehl aus, um zu überprüfen, ob das Symantec-Stamm-CA-Zertifikat vorhanden ist:

      ``# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

4.  Führen Sie diese Befehle aus, wenn die Datei nicht gefunden wird:

      ``# wget https://www.symantec.com/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

      ``# c_rehash``

5.  Führen Sie diesen Befehl aus, um einen symlink mit „b204d74a.0 -> VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem“ zu erstellen:

      ``# ln -s  VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0``

6.  Überprüfen Sie, ob dieser Befehl über die folgende Ausgabe verfügt. Falls nicht, müssen Sie einen symlink erstellen:

      ``# ls -l | grep Baltimore
      -rw-r--r-- 1 root root   1303 Apr  7  2016 Baltimore_CyberTrust_Root.pem
      lrwxrwxrwx 1 root root     29 May 30 04:47 3ad48a91.0 -> Baltimore_CyberTrust_Root.pem
      lrwxrwxrwx 1 root root     29 May 30 05:01 653b494a.0 -> Baltimore_CyberTrust_Root.pem``

7. Verwenden Sie diesen Befehl, um einen symlink zu erstellen, falls symlink 653b494a.0 nicht vorhanden ist:

      ``# ln -s Baltimore_CyberTrust_Root.pem 653b494a.0``


## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Ausgehende Konnektivität für Site Recovery-URLs oder IP-Bereiche (Fehlercode 151037 oder 151072)

Damit die Site Recovery-Replikation funktioniert, ist für die VM die ausgehende Konnektivität zu bestimmten URLs oder IP-Bereichen erforderlich. Wenn sich Ihre VM hinter eine Firewall befindet oder Netzwerksicherheitsgruppen-Regeln (NSG-Regeln) zum Steuern der ausgehenden Konnektivität verwendet werden, wird ggf. eine dieser Fehlermeldungen angezeigt:

**Fehlercodes** | **Mögliche Ursachen** | **Empfehlungen**
--- | --- | ---
151037<br></br>**Meldung**: Fehler beim Registrieren des virtuellen Azure-Computers bei Site Recovery. | - Sie verwenden die NSG zum Steuern des Zugriffs in ausgehender Richtung auf der VM, und die erforderlichen IP-Bereiche sind für den Zugriff in ausgehender Richtung nicht auf einer Positivliste aufgeführt.</br></br>- Sie verwenden Firewalltools von Drittanbietern, und die erforderlichen IP-Bereiche/URLs sind nicht auf einer Positivliste aufgeführt.</br>| - Wenn Sie einen Firewallproxy zum Steuern der ausgehenden Netzwerkkonnektivität auf der VM verwenden, sollten Sie sicherstellen, dass die erforderlichen URLs oder Datencenter-IP-Bereiche auf der Positivliste enthalten sind. Siehe hierzu die [Firewallproxy-Anleitung](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>- Wenn Sie NSG-Regeln zum Steuern der ausgehenden Netzwerkkonnektivität auf der VM verwenden, sollten Sie sicherstellen, dass die erforderlichen Datencenter-IP-Bereiche auf der Positivliste enthalten sind. Informationen hierzu unter [Networking guidance for replicating Azure virtual machines](https://aka.ms/a2a-nsg-guidance) (Netzwerkanleitung für die Replikation von virtuellen Azure-Computern).
151072<br></br>**Meldung**: Fehler bei der Site Recovery-Konfiguration. | Die Verbindung mit Site Recovery-Dienstendpunkten kann nicht hergestellt werden. | - Wenn Sie einen Firewallproxy zum Steuern der ausgehenden Netzwerkkonnektivität auf der VM verwenden, sollten Sie sicherstellen, dass die erforderlichen URLs oder Datencenter-IP-Bereiche auf der Positivliste enthalten sind. Siehe hierzu die [Firewallproxy-Anleitung](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>- Wenn Sie NSG-Regeln zum Steuern der ausgehenden Netzwerkkonnektivität auf der VM verwenden, sollten Sie sicherstellen, dass die erforderlichen Datencenter-IP-Bereiche auf der Positivliste enthalten sind. Informationen hierzu unter [Networking guidance for replicating Azure virtual machines](https://aka.ms/a2a-nsg-guidance) (Netzwerkanleitung für die Replikation von virtuellen Azure-Computern).

### <a name="fix-the-problem"></a>Beheben des Problems
Führen Sie die Schritte im [Dokument mit der Netzwerkanleitung](site-recovery-azure-to-azure-networking-guidance.md) aus, um die [erforderlichen URLs](site-recovery-azure-to-azure-networking-guidance.md#outbound-connectivity-for-azure-site-recovery-urls) bzw. die [erforderlichen IP-Bereiche](site-recovery-azure-to-azure-networking-guidance.md#outbound-connectivity-for-azure-site-recovery-ip-ranges) auf eine Positivliste zu setzen.

## <a name="disk-not-found-in-the-machine-error-code-150039"></a>Datenträger wurde auf dem Computer nicht gefunden (Fehlercode 150039)

Es muss ein neuer Datenträger initialisiert werden, der an die VM angefügt ist.

**Fehlercode** | **Mögliche Ursachen** | **Empfehlungen**
--- | --- | ---
150039<br></br>**Meldung**: Der Azure-Datenträger (DiskName) (DiskURI) mit der logischen Gerätenummer (LUN) (LUNValue) wurde keinem entsprechenden Datenträger zugeordnet, der vom virtuellen Computer gemeldet wird und denselben LUN-Wert aufweist. | - Es wurde ein neuer Datenträger an den virtuellen Computer angefügt, aber der Datenträger wurde nicht initialisiert.</br></br>- Der Datenträger innerhalb des virtuellen Computers meldet nicht den richtigen LUN-Wert, mit dem der Datenträger an den virtuellen Computer angefügt wurde.| Stellen Sie sicher, dass die Datenträger initialisiert wurden, und wiederholen Sie den Vorgang.</br></br>Für Windows: [Anfügen und Initialisieren eines neuen Datenträgers](https://docs.microsoft.com/azure/virtual-machines/windows/attach-disk-portal#option-1-attach-and-initialize-a-new-disk)</br></br>Für Linux: [Initialisieren eines neuen Datenträgers unter Linux](https://docs.microsoft.com/azure/virtual-machines/linux/classic/attach-disk#initialize-a-new-data-disk-in-linux)

### <a name="fix-the-problem"></a>Beheben des Problems
Stellen Sie sicher, dass die Datenträger initialisiert wurden, und wiederholen Sie den Vorgang:

- Für Windows: [Anfügen und Initialisieren eines neuen Datenträgers](https://docs.microsoft.com/azure/virtual-machines/windows/attach-disk-portal#option-1-attach-and-initialize-a-new-disk)
- Für Linux: [Initialisieren eines neuen Datenträgers unter Linux](https://docs.microsoft.com/azure/virtual-machines/linux/classic/attach-disk#initialize-a-new-data-disk-in-linux)

Wenden Sie sich an den Support, wenn das Problem weiterhin besteht.


## <a name="unable-to-see-the-azure-vm-for-selection-in-enable-replication"></a>Azure-VM wird für „Replikation aktivieren“ nicht zur Auswahl angezeigt

Ihre Azure-VM wird unter [Replikation aktivieren: Schritt 2](./site-recovery-azure-to-azure.md#step-2-select-virtual-machines) unter Umständen nicht angezeigt. Der Grund für dieses Problem kann eine veraltete Site Recovery-Konfiguration auf der Azure-VM sein. Die veraltete Konfiguration kann in den folgenden Fällen auf einer Azure-VM verbleiben:

- Sie haben die Replikation für die Azure-VM mithilfe von Site Recovery aktiviert und anschließend den Site Recovery-Tresor gelöscht, ohne die Replikation auf der VM explizit zu deaktivieren.
- Sie haben die Replikation für die Azure-VM mithilfe von Site Recovery aktiviert und anschließend die Ressourcengruppe mit dem Site Recovery-Tresor gelöscht, ohne die Replikation auf der VM explizit zu deaktivieren.

### <a name="fix-the-problem"></a>Beheben des Problems

Sie können das [Skript zum Entfernen veralteter ASR-Konfigurationen](https://gallery.technet.microsoft.com/Azure-Recovery-ASR-script-3a93f412) verwenden und die veraltete Site Recovery-Konfiguration auf der Azure-VM entfernen. Nach dem Entfernen der veralteten Konfiguration sollte die VM unter [Replikation aktivieren: Schritt 2](./site-recovery-azure-to-azure.md#step-2-select-virtual-machines) angezeigt werden.


## <a name="next-steps"></a>Nächste Schritte
[Replizieren virtueller Azure-Computer](site-recovery-replicate-azure-to-azure.md)

